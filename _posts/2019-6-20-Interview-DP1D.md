---
layout: post
title:  "Leetcode DP One dimension"
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

<hr>

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

<hr>

## 375. Guess Number Higher or Lower II
We can see that
1. If only one number, we don't need to guess, `cost = 0 `
2. If 2 number, we can guess the one that's smaller, `cost = smal one`
3. If 3 number, we can guess the middle one 
4. if more than 3, we can guess the one the not in the border

#### Iteration DP
```
class Solution {
public:
    int getMoneyAmount(int n) {
        //n+2 for border condition, dp[i][j] means from i to j
        vector<vector<int>> dp(n+2,vector<int>(n+2,0));
        // if only one number, then no need to guess, cost = 0
        // if two numbers, guess the small one
        // if three numbers, guess the middle one
        for(int len = 1; len <= n-1 ; len++ ){
            for(int j = 1 ; j <= n - len ; j++ ){
                //from j to j+len
                int GlobalMin = INT_MAX;
                for(int k = j+1; k < j+len ; k++){
                    int x = k + max(dp[j][k-1], dp[k+1][j+len]);
                    GlobalMin = min(GlobalMin, x);
                }
            dp[j][j+len] = len == 1? j : GlobalMin;
            }
        }        
        return dp[1][n];
    }
};
```
#### Recursive DP
```
class Solution {
public:
    int DFS(vector<vector<int>>&dp, int left,int right){
        if(left>=right) return 0 ;
        if(dp[left][right]) return dp[left][right];
        if(left+1 == right){
            dp[left][right] = left;
            return left;
        }
        int globalMin = INT_MAX;
        for(int k=left+1;k<right;k++){
            int x = k + max( DFS(dp,left,k-1), DFS(dp,k+1,right) );
            globalMin = min(globalMin, x);
        }
        dp[left][right] = globalMin;
        return globalMin;
    }
    
    int getMoneyAmount(int n) {
        //n+2 for border condition, dp[i][j] means from i to j
        vector<vector<int>> dp(n+2,vector<int>(n+2,0));
        // if only one number, then no need to guess, cost = 0
        // if two numbers, guess the small one
        // if three numbers, guess the middle one
        return DFS(dp,1,n);
    }
};
```

<hr>

## 312. Burst Balloons
[Blog solution](https://www.cnblogs.com/grandyang/p/5006441.html)
#### Iterative DP
```
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n = nums.size(); 
        if(n==0) return 0;
        // from 0 to n+1， insert 1 to front and back
        nums.insert(nums.begin(),1);
        nums.push_back(1);
        // we need dp[1][n]
        vector<vector<int>> dp(n+2,vector<int>(n+2,0));
        for(int len=0;len<=n;len++){
            for(int j=1;j<=n-len;j++){
                int globalMax = INT_MIN;
                for(int k=j;k<=j+len;k++){
                    int x = dp[j][k-1]+dp[k+1][j+len]+nums[k]*nums[j-1]*nums[j+len+1];
                    globalMax = max(globalMax, x);
                }
                dp[j][j+len] = globalMax;
            }
        }
        return dp[1][n];
    }
};
```
Same as last problem, DP in a interval. `DP[i][j]` means that all ballons are bursted in this interval. So, we need to make sure that `int x = dp[j][k-1]+dp[k+1][j+len]+nums[k]*nums[j-1]*nums[j+len+1];`, becasue when we calculate burst position k and add its neighbors, we need to make sure all its neighbors are actually bursted.

We try from `len=0` because we need to calculate single number situation.

And for k, `k=j and k = j+len`, because we are accessing `dp[k+1][j+len]`, and `k+1 > j+len`, then we can make sure its 0


#### Recursive DP
```
class Solution {
public:
    int DP(vector<int> &nums,vector<vector<int>>& dp,int left,int right){
        if(left > right) return 0;
        if(dp[left][right]) return dp[left][right];
        int globalMax = INT_MIN;
        for(int k=left;k<=right;k++){
            int x = nums[k]*nums[left-1]*nums[right+1]+DP(nums,dp,left,k-1)+DP(nums,dp,k+1,right);
            globalMax = max(globalMax,x);
        }
        dp[left][right] = globalMax;
        return globalMax;
    }
    int maxCoins(vector<int>& nums) {
        int n = nums.size(); 
        if(n==0) return 0;
        // from 0 to n+1， insert 1 to front and back
        nums.insert(nums.begin(),1);
        nums.push_back(1);
        vector<vector<int>> dp(n+2,vector<int>(n+2,0));
        return DP(nums,dp,1,n);
    }
};
```


## 322. Coin Change
This problem seems we can use greedy, but when `coins = {9, 6, 5, 1} and Value = 11`, greedy would fail.

This is a bag problem template usage, so we can use dp

#### Iterative
```
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        if(amount == 0) return 0;
        //dp[i] means number of coins
        vector<long long> dp(amount+2,INT_MAX-1);
        // we start from 0 to amount, and return dp[amount]
        dp[0] = 0;
        for(long long i=0;i<amount;i++){
            for(int j=0;j<coins.size();j++){
                if(i+coins[j] <= (long long)amount && dp[i] != INT_MAX-1){
                    dp[i+coins[j]] = min(dp[i]+1,dp[i+coins[j]]);
                }
            }
        }
        if(dp[amount] == INT_MAX-1) return -1;
        return dp[amount];
    }
};
```
