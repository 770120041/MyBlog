---
layout: post
title:  "Leetcode Mono stack"
categories: Interview
---
[blog](https://blog.csdn.net/liujian20150808/article/details/50752861)
[blog](https://www.cnblogs.com/grandyang/p/8887985.html)
[blog](https://zhanghuimeng.github.io/post/leetcode-84-largest-rectangle-in-histogram/)

## 84. Largest Rectangle in Histogram
#### O(n*n) space dp, MLE
使用一个二维数组来保存提前计算好的区间[i,j]内的最小值，那么预处理时间为O(n^2)，查询耗时O(1), 但是需要额外的O(n^2)空间，当数据量很大时，这个空间消耗是庞大的，而且当改变了数组中的某一个值时，更新二维数组中的最小值也很麻烦。
Simple dp solution, `dp[i][j]` is the minimum number between i,j, so the result would be 
`(j-i+1)*dp[i][j]`
```
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        // dp[i][j] is the minimum number in i,j
        int n = heights.size();
        vector<vector<int>> dp(n+1,vector<int>(n+1,INT_MAX));
        int result = 0;
        for(int len=0;len<=n;len++){
            for(int start=1;start+len<=n;start++){
                if(len == 0){
                    dp[start][start+len] = heights[start-1];
                }
                else{
                    dp[start][start+len] = min(heights[start+len-1],dp[start][start+len-1]);
                }
                result = max(result,(len+1)*dp[start][start+len]);
            }
        }
        return result;
    }
};
```
#### O(2*N) space dp TLE
It has 60000 numbers, so O(n^2) woudl be TLE
```
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        // dp[i][j] is the minimum number in i,j
        int n = heights.size();
        vector<vector<int>> dp(n+1,vector<int>(n+1,INT_MAX));
        int result = 0;
        for(int len=0;len<=n;len++){
            for(int start=1;start+len<=n;start++){
                if(len == 0){
                    dp[start][start+len] = heights[start-1];
                }
                else{
                    dp[start][start+len] = min(heights[start+len-1],dp[start][start+len-1]);
                }
                result = max(result,(len+1)*dp[start][start+len]);
            }
        }
        return result;
    }
};
```
#### Increasing Stack
为什么使用increasing stack，因为当后面的高度比前面的高的时候，前面的情况也会被后面的情况覆盖，所以只需要考虑每次`height[i]>height[i-1]`时候的情况。

由于使用了increasing stack，所以当`heights[i] <= heights[s.top()]`的时候，可以确定把stack的最后一个元素pop以后，`i-s.top()-1` 到i之间的所有元素都是大于`h[s.top()]`的，所以可以使用`i-s.top()-1`作为w的长度


这里push一个0到末尾可以更好处理边界条件，不需要在循环完清空栈，在栈头加上-1也是为了处理开始的边界条件
```
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        if(heights.size() == 0) return 0;
        int result = 0;
        heights.push_back(0);
        stack<int> s;
        s.push(-1);
        for(int i=0;i<heights.size();i++){
            while(s.top() != -1 and heights[i] <= heights[s.top()]){
                int h = heights[s.top()]; s.pop();
                int w = i-s.top()-1;
                //w滞后了,计算的时候是在h后面的位置。
                
                result = max(result,h*w);
            }
            s.push(i);
        }
        return result;
    }
};
```

<hr>