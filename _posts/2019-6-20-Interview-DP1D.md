---
layout: post
title:  "Leetcode DP One dimension"
categories: Interview
---
# DP symptoms
**DP 题目的特征：求最大或者最小的值（组合次数），不需要穷举。状态可以保留。多用于String或者搜索数组**


## 70. Climbing Stairs
`dp[i] = dp[i-1]+dp[i-2]+dp[i], and initially dp = 0 and set dp[1]=dp[2] = 1    `


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
DP or use math C(m,n) or use DFS to find all pathes
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
#### Brute Force DP
Doing this from bottom to top, use the original triangle
```
class Solution {
public:
    // do it reversely woudl be better
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        if(n == 0) return 0;
        for(int i=n-1;i>=1;i--){
            for(int j=1;j<triangle[i].size();j++){
                triangle[i-1][j-1]= triangle[i-1][j-1] + min(triangle[i][j],triangle[i][j-1]);
            }
        }
        return triangle[0][0];
    }
};
```

#### O(N) solution
[blog](https://www.cnblogs.com/grandyang/p/4286274.html)
```
class Solution {
public:
    // Save space for DP
    int minimumTotal(vector<vector<int>>& triangle) {
        vector<int> dp(triangle.back());
        for(int i=triangle.size()-2;i>=0;i--){
            // careful here must be from 0 to triangle[i].size()
            // otherwise it might get modified beforehand
            for(int j=0;j<triangle[i].size();j++){
                dp[j] = min(dp[j],dp[j+1])+triangle[i][j];
            }
        }
        return dp[0];
    }
};
```
<hr>

## 279. Perfect Squares
```
class Solution {
public:
    //完全背包问题。恰好容量为N
    int numSquares(int n) {
        vector<int> dp(n+1,INT_MAX);
        dp[0] = 0;
        for(int i=1;i*i<=n;i++){
            for(int j=i*i;j<=n;j++){
                dp[j] = min(dp[j],dp[j-i*i]+1);
            }
        }
        return dp[n];
    }
};
```

<hr>

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

#### A more clear Iterative DP 
The trick here is `dp[start][start+len] = min(dp[start][start+len], k+max(dp[start][k-1],dp[k+1][start+len]));`
```
class Solution {
public:
    int getMoneyAmount(int n) {
        vector<vector<int>> dp(n+1,vector<int>(n+1,INT_MAX/2));
        for(int i=1;i<=n;i++){
            dp[i][i] = 0;
            dp[i-1][i] = i-1;
        } 
        for(int len=1 ; len<=n ; len++){
            for(int start = 1;start<=n;start++){
                if(start+len <= n){
                    for(int k=start+1;k<start+len;k++){
                        dp[start][start+len] = min(dp[start][start+len], k+max(dp[start][k-1],dp[k+1][start+len]));
                    }
                }
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
        if(n == 0) return 0;
        nums.insert(nums.begin(),1);
        nums.push_back(1);
        vector<vector<int>> dp(n+2,vector<int>(n+2,0));
        for(int len=0;len<=n;len++){
            for(int j=1;j+len<=n;j++){
                int globalMax = INT_MIN;
                for(int k=j;k<=j+len;k++){
                    globalMax = max(globalMax, nums[k]*nums[j-1]*nums[j+len+1] + dp[j][k-1]+dp[k+1][j+len]);
                }
                dp[j][j+len] = globalMax;
            }
        }
        return dp[1][n];
    }
};
```
Same as last problem, DP in a interval. `DP[i][j]` means that all ballons are bursted in this interval. So, we need to make sure that `int x = dp[j][k-1]+dp[k+1][j+len]+nums[k]*nums[j-1]*nums[j+len+1];`, becasue when we calculate burst position k and add its neighbors, we need to make sure all its neighbors are actually bursted.

We try from `len=0` because we need to calculate single number situation. And we increase len so that each subproblem is solved first.

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
        //each coins cost 1, 完全背包问题
        int n = coins.size();
        vector<int> dp(amount+1,INT_MAX/2);
        dp[0] = 0;
        for(int i=0;i<coins.size();i++){
            for(int j=coins[i];j<=amount;j++){
                dp[j] = min(dp[j],dp[j-coins[i]]+1);
            }
        }
        return dp[amount] == INT_MAX/2?-1:dp[amount];
    }
};
```

<hr>

## 518. Coin Change 2
If no coins and amount = 0, there is 1 combination.

j starts from 0 such that `dp[coins[i]]` will be initialized.

If we do `dp[coins[i]] = 1`, it will make the result larger because `dp[i]` is added every time
#### confused solution
```
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        // number of combinations
        vector<int> dp(amount+1,0);
        dp[0] = 1;
        for(int i=0;i<coins.size();i++){
            for(int j=0;j<=amount-coins[i];j++){
                dp[j+coins[i]] += dp[j];
            }
        }
        return dp[amount];
    }
};
```


## More understandale solution
We only increment `dp[coins[i]]` when encounter this number.
```
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        // number of combinations
        vector<int> dp(amount+1,0);
        dp[0] = 1;
        for(int i=0;i<coins.size();i++){
            if(coins[i] <= amount)
                dp[coins[i]] ++;
            for(int j=1;j<=amount-coins[i];j++){
                dp[j+coins[i]] += dp[j];
            }
        }
        return dp[amount];
    }
};
```

<hr>

## 152. Maximum Product Subarray
Tricky DP
```
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        if(n == 0) return 0;
        vector<int> s(n,0),m(n,0);
        s[0] = m[0] = nums[0];
        int result = nums[0];
        for(int i=1;i<nums.size();i++){
            m[i] = max(max(nums[i],nums[i]*m[i-1]),nums[i]*s[i-1]);
            s[i] = min(min(nums[i],nums[i]*m[i-1]),nums[i]*s[i-1]);
            result = max(result,m[i]);
        }
        return result;
         
    }
};
```

<hr>