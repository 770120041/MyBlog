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
