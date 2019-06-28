---
layout: post
title:  "Leetcode DP01"
categories: Interview
---
 
 Using Simple Array to DP
## 55. Jump Game
Starts at 0, reaches at n(the length of array), note **not n-1**
```
bool canJump(vector<int>& nums) {
    int N = nums.size();
    int i=0;
    for(int reach=0;i<N && i<=reach;i++){
        reach  = max(reach,i+nums[i]);
    }
    return i==N;
}
```

<hr>


## 62. Unique Paths
DP or use math
```
int uniquePaths(int m, int n) {
    if(m <=1 || n <= 1) return 1;
    //TODO empty detection
    vector<vector<int>> dp;
    for(int i=0;i < m;i++){
        if(i == 0){
            vector<int> cur(n,1);
            dp.push_back(cur);
        }
        else{
            vector<int> cur(n,0);
            cur[0] = 1;
            dp.push_back(cur);
        }
    }
    for(int i=1;i<m;i++){
        for(int j=1;j<n;j++){
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
        }
    }
    return dp[m-1][n-1];
}
```

In total there are `m-1` horizontal steps and `n-1` vertical steps.

So the total number of steps is `m+n-2`, and the whole number to pick is `m-1` numbers for vertical

So answer is `C(m-1,m+n-2)`

```
//need m-1 + n-1 step
    //calculating C(N+M-2,M-1)
    
    int Combination(int n,int r){
        if(n-r<r){
            r = n-r;
        }
        //C(N,R) = N!/(R!*(N-R)!)
        long long up=1,down=1;
        for(int i=1;i<=r;i++){
            down *= i;
        }
        for(int i=n-r+1;i<=n;i++){
            up *= i;
        }
        return (int)(up/down);
    }
    
    int uniquePaths(int m, int n) {
        return Combination(m+n-2,m-1);  
    }
```

## 63. Unique Paths II
Simple DP, be careful for how to handle obstacles. 
* When init border cases, careful about how obstacles affect it
* When doing dp, we need to skip obstacles becasue we can neverr reach it

```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        vector<vector<unsigned long long>> dp;
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        if(m==0 || n==0) return 1;
        
        for(int i=0;i<m;i++){
            vector<unsigned long long> cur(n,0);
            dp.push_back(cur);
        }
         // no obstacles assign 1
        for(int i=0;i<m;i++){
            if(obstacleGrid[i][0]){
                break;
            }
            dp[i][0] = 1;
        }
        for(int j=0;j<n;j++){
            if(obstacleGrid[0][j]){
                break;
            }
            dp[0][j] = 1;
            
        }
        
        
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                if(!obstacleGrid[i][j]) dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```

## 120. Triangle
Be careful for border condition!
```
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n=triangle.size();
        if(n == 0){ return 0;}
        if(n == 1) return triangle[0][0];
        
        vector<vector<int>> dp;
        for(int i=0;i<n;i++){
            vector<int> cur(i+1,INT_MAX);
            dp.push_back(cur);
        }
        
        dp[0][0] = triangle[0][0];
        int minValue = INT_MAX;
        for(int i=1;i<n;i++){
            int m = dp[i].size();
            for(int j=0;j<m;j++){
                if(j == 0){
                    dp[i][j] = min(dp[i][j], dp[i-1][0] + triangle[i][j]);
                }
                else if(j == m-1){
                    dp[i][j] = min(dp[i][j], dp[i-1][m-2] + triangle[i][j]);
                }
                else{
                    dp[i][j] = min(dp[i][j], dp[i-1][j-1] + triangle[i][j]);
                    dp[i][j] = min(dp[i][j], dp[i-1][j] + triangle[i][j]);
                }
                
                // printf("i=%d,j=%d,dp[ij]=%d,trig[i,j]=%d\n",i,j,dp[i][j],triangle[i][j]);
                if(i == n-1){
                    minValue = min(minValue,dp[i][j]);
                }
            }
        }
        return minValue;
    }
};
```

## 139. Word Break
Brute force, but use an array to record the history of brute force.
```
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        int n = s.size();
        vector<bool> dp(n+1,false);
        //dp[i] 1到i之间可以可以break
        dp[0] = true;
        unordered_set<string> wordSet;
        for(auto s: wordDict){
            wordSet.insert(s);
        }
        for(int i=1;i<=n;i++){
            for(int j=0;j<i;j++){
                // 0-j is a break string  ,j-i is a break string
                if(dp[j]&& wordSet.count(s.substr(j,i-j))){
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[n];
    }
};
```

## 279. Perfect Squares
Dp[i] means the minimum number of perfect squres forming number i.

Must start from dp[0] = 0, we can validate that dp[i+j*j] = dp[4] = 1. so i need to be 0

The complexity is O(N*sqrt(N))
```
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1,INT_MAX);
        dp[0] = 0;
        for(int i=0;i<=n;i++){
            for(int j=1;i+j*j<=n;j++){
                dp[i+j*j] = min(dp[i+j*j],dp[i]+1);
            }
        }
        return dp[n];
    }
};
```

## 312. Burst Balloons
https://www.cnblogs.com/grandyang/p/5006441.html

## 375. Guess Number Higher or Lower II
How to judge `>= <=`?