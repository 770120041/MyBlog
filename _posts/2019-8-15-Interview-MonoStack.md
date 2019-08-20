---
layout: post
title:  "Leetcode Mono stack"
categories: Interview
---
[blog](https://blog.csdn.net/liujian20150808/article/details/50752861)
[blog](https://www.cnblogs.com/grandyang/p/8887985.html)
[blog](https://zhanghuimeng.github.io/post/leetcode-84-largest-rectangle-in-histogram/)
## Definiation
单调栈的定义：

单调栈就是栈内元素单调递增或者单调递减的栈，单调栈只能在栈顶操作。

为了更好的理解单调栈，则可将单调栈用生活情形模拟实现，例如：


我们借用拿号排队的场景来说明下。现在有很多人在排队买可乐，每个人手里都拿着号，越靠前的人手里的号越小，

但是号不一定是连续的。小明拿了号后并没有去排队，而是跑去约会了。等他回来后，发现队伍已经排得很长了，

他不能直接插入到队伍里，不然人家以为他是来插队的。小明只能跑到队伍最后，挨个询问排队人手里的号，

小明认为号比他大的人都是“插队”的，于是小明就会施魔法把这些人变消失，直到小明找到号比他小的为止。

在上面这个场景里，大家排的队伍就像是单调栈，因为大家手里拿的号是单调递增的。

而小明找自己位置的这个过程就是元素加入单调栈的过程。新加入的元素如果加到栈顶后，

如果栈里的元素不再是单调递增了，那么我们就删除加入前的栈顶元素，

就像小明施魔法把“插队”的人变消失一样。直到新元素加入后，栈依然是单调递增时，我们才把元素加进栈里。

（这样做的目的是“维护”单调栈，是单调栈保持原来的单调性不变）

从数组的角度阐述单调栈的性质：

给定一个包含若干个整数的数组，我们从第 1 个元素开始依次加入单调栈里，并且加入后更新单调栈。

那么单调栈有这样的性质：对于单调递增的栈，如果此时栈顶元素为 b，加入新元素 a 后进行更新时，

如果 a 大于 b，说明 a 在数组里不能再往左扩展了（由于单调栈的单调递增性质，b前面的元素均小于a），

也就是说，如果从 a 在数组中的位置开始往左边遍历，则 a 一定是第一个比 b 大的元素；

如果 a 小于 b，说明在数组里，a 前面至少有一个元素不能扩展到 a 的位置(至少有b元素，因为b的值要大于a，如果此时再加入新的

a，那么单调栈便不再单调，所以元素a此时不能压入栈顶，因为这并不是元素a"应该"在的位置，只有当元素a找到自己的位置时

元素a方能压入栈中，而这样做的前提是不改变单调栈的单调性)，也就是对于这些元素来说，a 是其在数组右侧第一个比它小的元素。

**单调栈的维护是 O(n) 级的时间复杂度，因为所有元素只会进入栈一次，并且出栈后再也不会进栈了。**

单调栈的性质：

1.单调栈里的元素具有单调性

2.元素加入栈前，会在栈顶端把破坏栈单调性的元素都删除

**3.使用单调栈可以找到元素向左遍历第一个比他小的元素，也可以找到元素向左遍历第一个比他大的元素。**

对于第三条性质的解释（最常用的性质）：

对于单调栈的第三条性质，你可能会产生疑问，为什么使用单调栈可以找到元素向左遍历第一个比他大的元素，

而不是最后一个比他大的元素呢？我们可以从单调栈中元素的单调性来解释这个问题，由于单调栈中的元素只能是单调递增或者是单调

递减的，所以我们可以分别讨论这两种情况（假设不存在两个相同的元素）：

1.当单调栈中的元素是单调递增的时候，根据上面我们从数组的角度阐述单调栈的性质的叙述，可以得出：

（1）.当a > b 时，则将元素a插入栈顶，新的栈顶则为a

（2）.当a < b 时，则将从当前栈顶位置向前查找（边查找，栈顶元素边出栈），直到找到第一个比a小的数，停止查找，将元素a

插入栈顶（在当前找到的数之后，即此时元素a找到了自己的“位置”）

2.当单调栈中的元素是单调递减的时候，则有：


（1）.当a < b 时，则将元素a插入栈顶，新的栈顶则为a

（2）.当a > b 时，则将从当前栈顶位置向前查找（边查找，栈顶元素边出栈），直到找到第一个比a大的数，停止查找，将元素a

插入栈顶（在当前找到的数之后，即此时元素a找到了自己的“位置”）


 ———————————————— 
版权声明：本文为CSDN博主「Adherer」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/liujian20150808/article/details/50752861


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

**使用单调栈可以找到元素向左遍历第一个比他小的元素，也可以找到元素向左遍历第一个比他大的元素。**
这里使用这个性质，找到第一个比栈顶height小的元素，然后获取以栈顶高度为height的元素的w，和result比较得出结果
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

Mistakes:
Special examples:
`[2]  [2,1,2]` 
1. ` heights[s.top()] >= heights[i]`
最好是单调递增， 否走的话，无法覆盖到正确的范围。覆盖的范围会变小。


2. The order between `int w = i - s.top() - 1;` and `int h = heights[s.top()]; s.pop()`
The reason why firstly pop then calculate w is becasue the stack is increasing, so it shouldn't cal h first otherwise it will be smaller. For case [2,1,2], the stack would be `[-1] [-1,0] [-1,1] [-1,1,2] [-1]`, which can get the maximum result.

<hr>

## 901. Online Stock Span
Decreasing stack

解题方法
单调递减栈
看了数值的范围，可以肯定这个题的时间复杂度必须在O(n)以内，也就是说平均每次next()方法调用的时候，必须在将近O(1)的时间内找到前面多少天的价格是小于等于今天的。

这个题的重点在于连续二字上，我们只需要向前找到第一个比当前数字大的位置就停止。那么我们只需要找到数字A其前面有多少个连续的并且比它小的数字个数a即可，这样，当我们后面出现一个数字B，当B>=A时，在B前面小于等于B的连续数字共有a + 1个；当B < A时，在B前面小于等于B的连续数字只有1个，那就是B自己。

思路是使用一个单调递减栈，这个栈里保存的是当前的价格向前可以找连续的多少天。注意这个栈里存放的内容是严格单调递减的，如果新来的数值大于了栈顶元素，那么就要把栈顶的元素给弹出去，直到当前元素小于栈顶才行。

这样做的好处就是，我们没必要保留较小的元素，只需要知道每个元素前面有几个比它小的数字就行了。因为我们在遍历的过程中，是在找比当前元素小的元素个数，栈顶保留的只有较大的元素和它前面出现的次数，那么就知道了前面比它小的元素个数。

 ———————————————— 
版权声明：本文为CSDN博主「负雪明烛」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/fuxuemingzhu/article/details/82781059
```
class StockSpanner {
public:
    stack<pair<int,int>> s;
    StockSpanner() {
    }
    
    int next(int price) {
        int result = 1;
        while(s.size() and s.top().first <= price){
            result += s.top().second;
            s.pop();
        }
        s.push({price,result});
        return result;
    }
};

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner* obj = new StockSpanner();
 * int param_1 = obj->next(price);
 */
 ```

<hr>

## 739. Daily Temperatures
#### Decreasing Stack
题目:对于每个位置，找到第一个比它大的位置的
```
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        stack<pair<int,int>> s;
        vector<int> result(T.size(),0);
        for(int i=0;i<T.size();i++){
            while(!s.empty() and s.top().first < T[i]){
                result[s.top().second] = i - s.top().second;
                s.pop();
            }
            s.push({T[i],i});
        }
        return result;
    }
};
```

<hr>