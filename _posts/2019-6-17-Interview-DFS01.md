---
layout: post
title:  "Leetcode DFS 01 "
categories: Interview
---
## 77. Combinations

#### Iterative:
```
class Solution {
public:
    void DFS(vector<vector<int>>&res,vector<int>&cur,int n,int k,int next){
        if(cur.size() == k){
            res.push_back(cur);
        }
        for(int i=next;i<=n;i++){
            cur.push_back(i);
            DFS(res,cur,n,k,i+1);
            cur.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> result;
        vector<int> cur;
        DFS(result,cur,n,k,1);
        return result;
    }
};
```
<hr>

## 39. Combination Sum
Similiar, but each number could appear multiple times, we need to make sure to check this.

Best illustates DFS: take it and not take it.
```
class Solution {
public:
    void DFS(vector<vector<int>> &result,vector<int> cur,int target,vector<int>& candidates,int curSum,int curIndex){
        if(curSum == target){
            result.push_back(cur);
            return;
        }
        if(curSum>target){
            return;
        }
        // not take this position
        if(curIndex+1<candidates.size()){
            DFS(result,cur,target,candidates,curSum,curIndex+1);
        }
        // take this position
        if(curSum+candidates[curIndex] <= target){
            cur.push_back(candidates[curIndex]);
            DFS(result,cur,target,candidates,curSum+candidates[curIndex],curIndex);
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> result;
        vector<int> tmp;
        DFS(result,tmp,target,candidates,0,0);
        return result;
    }
};
```

```
class Solution {
public:
    //not vector<int> &cur here
    void DFS(vector<vector<int>>& result,vector<int>&candidates,int target,vector<int>cur,int sum,int index){
        // printf("sum=%d,index=%d\n",sum,index);
        // for(int i=0;i<cur.size();i++){
        //     cout<<cur[i]<<" ";
        // }
        // cout<<endl;
        if(sum == target){
            result.push_back(cur);
            return;
        }
        if(sum > target or index >= candidates.size()){
            return;
        }
        for(int i=0;i< (target-sum)/candidates[index]+1 ; i++){
            DFS(result,candidates,target,cur,sum+i*candidates[index],index+1);
            cur.push_back(candidates[index]);
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> result;
        vector<int> cur;
        
        DFS(result,candidates,target,cur,0,0);
        return result;
    }
};
```

## 299. Bulls and Cows
Simple usage of unordered_hashmap

```
string getHint(string secret, string guess) {
    int bulls=0 ,cows=0;
    unordered_map<char,int> secretCount;
    unordered_map<char,int> guessCount;
    for(int i=0;i<secret.size();i++){
        if(guess[i] == secret[i]){
            bulls++;
        }
        else{
            if(secretCount.count(secret[i])){
                secretCount[secret[i]]++;
            }
            else{
                secretCount[secret[i]] = 1;
            }
            if(guessCount.count(guess[i])){
                guessCount[guess[i]]++;
            }
            else{
                guessCount[guess[i]] = 1;
            }
        }
    }
    for(auto it=guessCount.begin(); it != guessCount.end() ; it++){
        cout<<it->first<<" "<<it->second<<endl;
        if(secretCount.count(it->first)){
            cows += min(secretCount[it->first],it->second);
        }
    }
    return to_string(bulls)+"A"+to_string(cows)+"B";
}
```
<hr>


## 200. Number of Islands

```
class Solution {
public:
    int xneighbor[4] = {1,-1,0,0};
    int yneighbor[4] = {0,0,1,-1};
    void DFS(vector<vector<char>>& grid,int x,int y,int n,int m){
        if(grid[x][y] == '0') return;
        grid[x][y] = '0';
        for(int i=0;i<4;i++){
            int xNext = x+xneighbor[i];
            int yNext = y+yneighbor[i];
            if(xNext<0 || xNext>=n || yNext<0 || yNext>=m ){
                continue;
            }
            if(grid[xNext][yNext] == '1'){
                DFS(grid,xNext,yNext,n,m);
            }
        }
    }
    
    int numIslands(vector<vector<char>>& grid) {
        int n = grid.size();
        if(n==0) return 0;
        int m = grid[0].size();
        if(m==0) return 0;
        
        vector<vector<char>> isvisited(grid);
        int result = 0;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(isvisited[i][j] == '1'){
                    result ++;
                    DFS(isvisited,i,j,n,m);
                }
            }
        }
        return result;
    }
};
```

## 130. Surrounded Regions
Similiar to Question 200, but this time we need to transform all islands who is not connected to edges. So one way is to search all islands, and if it connected to edges, then roll back(also use a bool to remember if visited). But it will be easier if we scan all edge islands first, and mark all islands who is connected to edges as `M`, then it will be as simple as question 200
```
class Solution {
public:
    void DFS(vector<vector<char>>& board,int i,int j,int m,int n,char target ){
        if(i<0 or i >=m or j<0 or j>=n or board[i][j] == 'X' or board[i][j] == 'M'){
            return ;
        }
        board[i][j] = target;
        DFS(board,i+1,j,m,n,target);
        DFS(board,i-1,j,m,n,target);
        DFS(board,i,j+1,m,n,target);
        DFS(board,i,j-1,m,n,target);
    }
    void solve(vector<vector<char>>& board) {
        int m = board.size();
        if(m==0) return;
        int n = board[0].size();
        if(n==0) return;
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                // only scan edges
                if(i == 0 or i == m-1 or j==0 or j == n-1){
                    if(board[i][j] == 'O'){
                        DFS(board,i,j,m,n,'M');
                    }
                }
            }
        }
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                // only scan edges
                if(board[i][j] == 'O'){
                    DFS(board,i,j,m,n,'X');
                }
            }
        }
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                // only scan edges
                if(board[i][j] == 'M'){
                    board[i][j] = 'O';
                }
            }
        }
    }
};
```