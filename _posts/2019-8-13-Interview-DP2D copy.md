---
layout: post
title:  "Leetcode DP 2D"
categories: Interview
---
## 256 Paint House	(lint.515)
#### Easy dp
```
class Solution {
public:
    /**
     * @param costs: n x 3 cost matrix
     * @return: An integer, the minimum cost to paint all houses
     */
    using vi = vector<int>;
    using vvi = vector<vi>;
    int minCost(vector<vector<int>> &costs) {
        // dp[i,j],the min cost of each postion paint color j at pos i
        int n = costs.size();
        if(n == 0) return 0;
        vvi dp(n+1,vi(3,INT_MAX));
        dp[0][0] = dp[0][2] = dp[0][1] = 0;
        for(int i=1;i<=n;i++){
            for(int j=0;j<3;j++){
                for(int k=0;k<3;k++){
                    if(k == j) continue;
                    dp[i][j] = min(dp[i][j] , dp[i-1][k] + costs[i-1][j]);
                }
            }
        }
        return min(dp[n][0],min(dp[n][1],dp[n][2]));
    }
};
```

<hr>

## 265 Paint House II(Lint 516)
#### Brute force O(N*K*K) TLE
```
class Solution {
public:
    /**
     * @param costs: n x k cost matrix
     * @return: an integer, the minimum cost to paint all houses
     */
    using vi = vector<int>;
    using vvi = vector<vi>;
    int minCostII(vector<vector<int>> &costs) {
        int n = costs.size();
        if(n == 0) return 0;
        int k = costs[0].size();
        vvi dp(n+1,vi(k,INT_MAX/2));
        for(int j=0;j<k;j++) dp[0][j] = 0;
        for(int i=1;i<=n;i++){
            for(int j=0;j<k;j++){
                for(int m=0;m<k;m++){
                    if(m == j) continue;
                    dp[i][j] = min(dp[i][j],dp[i-1][m]+costs[i-1][j]);
                }
            }
        }
        int result = dp[n][0];
        for(int i=1;i<k;i++) result = min(result,dp[n][i]);
        return result;
    }
};
```

#### Passed O(N*k)
Actually don't need to go through to find the minimum, can do it with only 3 variables
```
class Solution {
public:
    /**
     * @param costs: n x k cost matrix
     * @return: an integer, the minimum cost to paint all houses
     */
    using vi = vector<int>;
    using vvi = vector<vi>;
    int minCostII(vector<vector<int>> &costs) {
        int n = costs.size();
        if(n == 0) return 0;
        int k = costs[0].size();
        vvi dp(n+1,vi(k,INT_MAX/2));
        for(int j=0;j<k;j++) dp[0][j] = 0;
        //O(N*2K)
        for(int i=1;i<=n;i++){
            int min1=INT_MAX,min2=INT_MAX;
            int min1Index = -1;
            for(int j=0;j<k;j++){
                //store the minimum value to dp[i-1][j]
                dp[i][j] = dp[i-1][j]+costs[i-1][j];
                if(dp[i][j] <= min1){
                    min2 = min1;
                    min1Index = j;
                    min1 = dp[i][j];
                } 
                else if(dp[i][j] <= min2) min2 = dp[i][j];
            }
            for(int j=0;j<k;j++){
                if(j == min1Index) dp[i][j] = min2;
                else dp[i][j] = min1;
            }
        }
        int result = dp[n][0];
        for(int i=1;i<k;i++) result = min(result,dp[n][i]);
        return result;
    }
};
```
<hr>

## 64. Minimum Path Sum
#### Template dp 
```
class Solution {
public:
    using vi = vector<int>;
    using vvi = vector<vi>;
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        if(m == 0) return 0;
        int n = grid[0].size();
        if(n == 0) return 0;
        vvi dp(m+1,vi(n+1,INT_MAX));
        dp[0][1]=dp[1][0] = 0;
        
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                dp[i][j] = min(dp[i-1][j],dp[i][j-1]) + grid[i-1][j-1];
            }
        }
        return dp[m][n];
    }
};
```

<hr>

## 72. Edit Distance
Leetcode上好多动态规划的题 ， 一般碰到这样的题，我第一眼是懵逼了，然后深呼吸一下，先用递归把做出来，结果显然，TLE。既然能用递归，就肯定可以用动态规划，然后就把递归变成动态规划，变成动态规划后看能不能再优化空间，如果不能，就直接return了，如果能就优化，这个现在已经成为套路了。

#### Recursion TLE
```
class Solution {
public:
    int minDistance(string word1, string word2) {
        return helper(word1,0,word2,0);
    }
    int helper(const string&word1,int i,const string&word2,int j){
        if(i == word1.size()) return int(word2.size())-j;
        if(j == word2.size()) return int(word1.size())-i;
        if(word1[i] == word2[j]){
            return helper(word1,i+1,word2,j+1);
        }
        else{
            int insertChar = helper(word1,i,word2,j+1);
            int delChar = helper(word1,i+1,word2,j);
            int editChar =  helper(word1,i+1,word2,j+1);
            int result = min(editChar,min(insertChar,delChar)) + 1;
            return result;
        }
    }
};
```

#### Recursion with memo
```
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> memo(word1.size(),vector<int>(word2.size(),0));
        return helper(word1,0,word2,0,memo);
    }
    int helper(const string&word1,int i,const string&word2,int j,vector<vector<int>> &memo){
        if(i == word1.size()) return int(word2.size())-j;
        if(j == word2.size()) return int(word1.size())-i;
        if(memo[i][j]) return memo[i][j];
        if(word1[i] == word2[j]){
            return helper(word1,i+1,word2,j+1,memo);
        }
        int insertChar = helper(word1,i,word2,j+1,memo);
        int delChar = helper(word1,i+1,word2,j,memo);
        int editChar =  helper(word1,i+1,word2,j+1,memo);
        int result = min(editChar,min(insertChar,delChar)) + 1;
        return memo[i][j] = result;
    }
};
```
#### DP
Fill in the memo in recursive solution

<hr>