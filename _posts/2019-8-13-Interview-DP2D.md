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

#### Recursion with memo(Recursive DP)
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
#### Iterative DP
`dp[i][j]` means the minimum edit numbers for `word[0..i]` to `word[0..j]`
assume word1 has size m and word2 has size n, we initialize the dp array to size(m+1)*(n+1) for better handling of border conditions.

We initialize the dp array using `dp[0][i] = dp[j][0] = 1` because if word1 or word2 is empty, then the minimum edit operations is to remove or add
```
dp[i][j] =  dp[i-1][j-1] if word1[i] == word2[j]
         =  1 + min(dp[i-1][j-1],dp[i,j-1],dp[i-1,j]) edit,remove and insert
```

```
class Solution {
public:
    using vi = vector<int>;
    using vvi = vector<vi>;
    int minDistance(string word1, string word2) {
        int n = word1.size();
        int m = word2.size();
        vector<vector<int>> dp(n+1,vector<int>(m+1,INT_MAX/2));
        for(int i=0;i<=m;i++) dp[0][i] = i;
        for(int i=0;i<=n;i++) dp[i][0] = i;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(word1[i-1] == word2[j-1]) dp[i][j] = dp[i-1][j-1];
                else{
                    dp[i][j] = 1 + min(dp[i-1][j],min(dp[i-1][j-1],dp[i][j-1]));
                }
            }
        }
        return dp[n][m];
    }
};
```

<hr>

## 97. Interleaving String
#### Brute Force Recursive  
First  idea is to brute force recursive
```
class Solution {
public:
    bool DFS(const string&s1,int i,const string&s2,int j,const string&s3){
        if(i + j == s3.size()) return true;
        if(i != s1.size()){
             if(s1[i] == s3[i+j]){
                 if(DFS(s1,i+1,s2,j,s3)) return true;
             } 
        }
        if(j != s2.size()){
            if(s2[j] == s3[i+j]){
                if(DFS(s1,i,s2,j+1,s3)) return true;
            }
        }
        return false;
    }
    bool isInterleave(string s1, string s2, string s3) {
        return DFS(s1,0,s2,0,s3);
    }
};
```

#### Recursive with Memo
```
class Solution {
public:
    bool DFS(const string&s1,int i,const string&s2,int j,const string&s3,vector<vector<bool>>& dp){
        if(i == s1.size() and j == s2.size()) return true;
        if(i < s1.size()){
             if(s1[i] == s3[i+j]){
                 if(dp[i+1][j] and DFS(s1,i+1,s2,j,s3,dp)) return true;
                 dp[i+1][j] = false;
             } 
        }
        if(j < s2.size()){
            if(s2[j] == s3[i+j]){
                if(dp[i][j+1] and DFS(s1,i,s2,j+1,s3,dp)) return true;
                dp[i][j+1] = false;
            }
        }
        return false;
    }
    bool isInterleave(string s1, string s2, string s3) {
        int n = s1.size();
        int m = s2.size();
        if(n+m != s3.size()) return false;
        //dp needs to be larger because it may go out of the border
        vector<vector<bool>> dp(n+1,vector<bool>(m+1,true));
        return DFS(s1,0,s2,0,s3,dp);
    }
};
```

#### Iterative DP

```
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int m = s1.size();
        int n = s2.size();
        if(m+n != s3.size()) return false;
        // dp[i][j] means if using 1...i from s1 and 1..j from s2 can make up i+j in s3
        vector<vector<bool>> dp(m+1,vector<bool>(n+1,0));
        dp[0][0] = true;
        for(int i=1;i<=m;i++) dp[i][0] = (dp[i-1][0] and s1[i-1] == s3[i-1]);
        for(int i=1;i<=n;i++) dp[0][i] = (dp[0][i-1] and s2[i-1] == s3[i-1]);
        
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(s1[i-1] == s3[i-1+j] and  dp[i-1][j]) dp[i][j] = true;
                if(s2[j-1] == s3[i+j-1] and dp[i][j-1]) dp[i][j] = true;
            }
        }
    
        return dp[m][n];
    }
};
```
 
<hr>

## 174. Dungeon Game

#### DP
`Spacce O(n*m) Time O(n*m)`

The tricky part is that each position the minimum hp is 1. And it is much easier to do it from the prince to the knight. Also for the bottom-right position, to make sure it can reach it we need to set the hp of the border of it to 1
```
class Solution {
public:
    int calculateMinimumHP(vector<vector<int>>& dungeon) {
        int m = dungeon.size();
        if(m == 0) return 0;
        int n = dungeon[0].size();
        // dp[i][j] is the number minimum hp required to reach m-1,n-1 from i,j
        vector<vector<int>> dp(m+1,vector<int>(n+1,INT_MAX));
        // each place minimum hp is 1
        // reach the bottom right needs 1, so this palce is 1
        // 保证至少要有1，利用这个位置来设置公主的位置，因为公主的位置要先考虑这两个位置
        dp[m][n-1] = dp[m-1][n] = 1;
        for(int i=m-1;i>=0;i--){
            for(int j=n-1;j>=0;j--){
                int val = min(dp[i+1][j], dp[i][j+1])-dungeon[i][j];
                dp[i][j] = max(val, 1);
            }
        }
        return dp[0][0];
    }
};
```

#### O(n) Space DP
Because it only uses two levels, so we can always optimize the space to `O(2*N)` and `O(N)`

```
class Solution {
public:
    int calculateMinimumHP(vector<vector<int>>& dungeon) {
        int m = dungeon.size();
        if(m == 0) return 0;
        int n = dungeon[0].size();
        // dp[j] is the number minimum hp required to reach m-1,n-1 from i,j
        vector<int> dp(n+1,INT_MAX);
        // each place minimum hp is 1
        // reach the bottom right needs 1, so this palce is 1
        // 特别注意这里dp[n]=1会错，因为dp[n]=1会导致每次循环到dp[n-1]=max(dp[n-1],dp[n])的时候都用到了dp[n]=1,会导致结果小
        dp[n-1] = 1;
        for(int i=m-1;i>=0;i--){ 
            for(int j=n-1;j>=0;j--){
                int val = min(dp[j], dp[j+1])-dungeon[i][j];
                //if dp[j] <= 0, it means that the hp bonuse can cover all the cost from i,j to m-1,n-1
                dp[j] = max(val, 1);
            }
        }
        return dp[0];
    }
};
```

<hr>

## 221. Maximal Square
```
dp[i][j] = 0 if (matrix[i][j] == 0)

         = min(dp[i-1][j],dp[i-1][j-1],dp[i][j-1])+1

```
#### DP Solution
``` 
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int m = matrix.size();
        if(m == 0) return 0;
        int n = matrix[0].size();
        vector<vector<int>> dp(m+1,vector<int>(n+1,0));
        int result = 0;
        for(int i=1;i<=m;i++){
            for(int j=1;j<=n;j++){
                if(matrix[i-1][j-1]=='1'){
                    dp[i][j] = min(min(dp[i-1][j],dp[i][j-1]),dp[i-1][j-1])+1;
                }
                else{
                    dp[i][j] = 0;
                }
                result = max(result,dp[i][j]*dp[i][j]);
            }
        }
        return result;
    }
};
```
<hr>

## 85. Maximal Rectangle
The biggest difference is that for a rectangle, it has many different shapes, while for squre, it has only one shape. So if we only record the maximum size for each position, it is not enough.

<hr>