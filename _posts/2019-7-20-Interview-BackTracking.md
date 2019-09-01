---
layout: post
title:  "Leetcode BackTracking"
categories: Interview
---
## 78. Subsets
#### Enumerate by Increasing the Index.

Time Complexity O(2^N)(minimum because the result is 2^N)
Space Complexity: O(N)(minimum)
```
void DFS(vector<vector<int>>&result,vector<int>& cur,vector<int>&nums,int index){
    result.push_back(cur);
    if(index >= nums.size()) return;
    for(int i = index;i<nums.size();i++){
        cur.push_back(nums[i]);
        DFS(result,cur,nums,i+1);
        cur.pop_back();
    }
}
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> empty;
    DFS(result,empty,nums,0);
    return result;
}
```

<hr>

## 90. Subsets 2
The difficulity caused by duplicates is that

`[1,2,2,2]`, so`[1,2,2] and [1,2,2]` are same 

The way to solve this is that in the same pass, we don't add the same number for 2 times. So just sort first then each time, we avoid adding duplicate numbers by skipping duplicate numbers. So if there are 5 duplicate numbers, after soring they are adjacent, using this trick we will add duplicate numebrs zero times, once and up to 5 times, which enumerates all cases.

```
while(i+1<nums.size() and nums[i+1] == nums[i]){
    i++;
}
```
#### Tricky Solution
```
void DFS(vector<vector<int>>&result,vector<int>&nums,vector<int> &cur,int index){
    result.push_back(cur);
    if(index >= nums.size()) return;
    for(int i=index;i<nums.size();i++){
        cur.push_back(nums[i]);
        DFS(result,nums,cur,i+1);
        cur.pop_back();
        while(i+1<nums.size() and nums[i+1] == nums[i]){
            i++;
        }
    }
}
vector<vector<int>> subsetsWithDup(vector<int>& nums) {
    sort(nums.begin(),nums.end());
    vector<vector<int>> result;
    vector<int> empty;
    DFS(result,nums,empty,0);
    return result;
}
```

<hr>

## 77.Combinations
Same as subsets I, but cut branch earlier
```
class Solution {
public:
    //Same as subsets
    void DFS(vector<vector<int>> &result, vector<int>&cur, int n,int k, int curIndex){
        if(cur.size() == k){
            result.push_back(cur);
            return;
        }
        for(int i=curIndex;i<=n;i++){
            cur.push_back(i);
            DFS(result,cur,n,k,i+1);
            cur.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        vector<int> tmp;
        vector<vector<int>> result;
        DFS(result,tmp,n,k,1);
        return result;
    }
};
```

<hr>

## 39. Combination Sum

#### Brute Force
Two border situations: `curSum>target and curIndex>=nums.size()`
```
class Solution {
public:
    using vi = vector<int>;
    using vvi = vector<vector<int>>;
    void DFS(vvi&result,vi&nums,vi&cur,int target,int curIndex,int curSum){
        if(curSum >= target or curIndex>=nums.size()){
            if(curSum == target){
                result.push_back(cur);
            }
            return;
        }
        int maxTimes = (target-curSum)/nums[curIndex]+1;
        for(int j=0;j<=maxTimes;j++){
            DFS(result , nums , cur , target , curIndex+1 , curSum+nums[curIndex]*j);
            cur.push_back(nums[curIndex]);
        }
        for(int j=0;j<=maxTimes;j++){
            cur.pop_back();
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vvi result;
        vi cur;
        DFS(result,candidates,cur,target,0,0);
        return result;
    }
};
```

<hr>

## 40. Combination Sum II
Same as Subsets 2, List all sets and find `if sum == target`
```
class Solution {
public:
    using vi = vector<int>;
    using vvi = vector<vi>;
    void DFS(vvi&result,vi&cur,vi&nums,int target,int curSum,int curIndex){
        if(curIndex>= nums.size() or curSum >= target){
            if(curSum == target){
                result.push_back(cur);
            }
            return;
        }
        for(int j=curIndex;j<nums.size();j++){
            cur.push_back(nums[j]);
            DFS(result,cur,nums,target,curSum+nums[j],j+1);
            cur.pop_back();
            while(j<nums.size()-1 and nums[j+1] == nums[j]){
                j++;
            }
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        vvi result;
        vi cur;
        DFS(result,cur,candidates,target,0,0);
        return result;
    }
};
```

<hr>

## 216. Combination Sum III
Same as 39, but restrict the element number of resulting set.
```
class Solution {
public:
    using vi = vector<int>;
    using vvi = vector<vi>;
    void DFS(vvi &result, vi& cur, int k, int n, int curIndex, int curSum){
        if(curIndex>9 or curSum >= n){
            if(curSum == n and cur.size() == k ){
                result.push_back(cur);
            }
            return;
        }
        // use cur number or not use cur numebr
        DFS(result,cur,k,n,curIndex+1,curSum); 
        
        cur.push_back(curIndex);
        DFS(result,cur,k,n,curIndex+1,curSum+curIndex);            
        cur.pop_back();        
    }
    vector<vector<int>> combinationSum3(int k, int n) {
        vvi result;
        vi tmp;
        DFS(result,tmp,k,n,1,0);
        return result;
    }
};
```

## 377. Combination Sum IV


Climbing stairs, but the edge cases are: not reachable postion

#### Iterative(complicated)
And need to use unsigned long long
```
class Solution {
public:
    using LONG = unsigned long long;
    bool reachable(vector<int>&nums,int target){
        vector<int> dp(target+2,0);
        for(int i=0;i<nums.size();i++){
            if(nums[i]<=target){
                dp[nums[i]] = 1;            
            }
        }
        for(int i=0;i<=target;i++){
            for(int j=0;j<nums.size();j++){
                // dp[i] = dp[i] + dp[i-steps[j]]
                if(dp[i] == 0 and i-nums[j]>=0 and dp[i-nums[j]]){
                    dp[i] = 1;
                }
            }
        }
        return dp[target] == 1;
    }
    // How to DP? Same as climbing chairs
    int combinationSum4(vector<int>& nums, int target) {
        if(target == 0) return 1;
        vector<LONG> dp(target+2,0);
        for(int i=0;i<nums.size();i++){
            if(nums[i]<=target){
                dp[nums[i]] = 1;            
            }
        }
        if(reachable(nums,target)){
            for(int i=0;i<=target;i++){
                for(int j=0;j<nums.size();j++){
                    // dp[i] = dp[i] + dp[i-steps[j]]
                    if(i-nums[j]>=0){
                        dp[i] += dp[i-nums[j]];
                    }
                }
            }
            return dp[target];
        }
        // this numebr is not reachable
        return 0;
    }
};
```

#### Recursive
Recursive is much better because it only go over necessary points and don't need to solve edge cases.
```
class Solution {
public:
    using vi = vector<int>;
    using vb = vector<bool>;
    int DFS(vi&dp,vb& visited,vi&nums,int curIndex){
        if(curIndex<0) return 0;
        if(visited[curIndex]) return dp[curIndex];
        visited[curIndex] = true;
        for(int i=0;i<nums.size();i++){
            dp[curIndex] += DFS(dp,visited,nums,curIndex-nums[i]);
        }
        return dp[curIndex];
    }
    int combinationSum4(vector<int>& nums, int target) {
        if(target == 0) return 1;
        vi dp(target+2,0);
        vb visited(target+2,false);
        for(int i=0;i<nums.size();i++){
            if(nums[i] <= target){
                dp[nums[i]]=1;
            }
        }
        return DFS(dp,visited,nums,target);
    }
};
```

<hr>

#### Follow up:
What if negative numbers are allowed in the given array?
How does it change the problem?
What limitation we need to add to the question to allow negative numbers?

Answer:
If there are negative numbers, we need to guaranteen that any combinations of 
elements in this array can't reach this negative number. Otherwise we would have 
infinite combinations

Example:
```
[1,2,-3]
4

because 1+2 = -3, then we can loop here
```

## 254. Factor Combinations	(Lintcode:1308)
Just back trakcing, no factor problem.

The tricky part is that for DFS, next time we need to start from i ` DFS(result,cur,n/i,i`
```
using vi =  vector<int>;
using vvi = vector<vi>;
void DFS(vvi&result,vi&cur,int n,int start){
    if(n == 1){
        //prevent the only element is n itself
        if(cur.size() > 1)
            result.push_back(cur);
        return ;
    }
    for(int i=start;i<=n;i++){
        if(n%i==0){
            cur.push_back(i);
            // starts from i, the reason is that any numbers start before i has been scanned
            // for example: 8/4 =2, but we need to start from 4, not 2, because it will cause duplication
            cout<<n<<" "<<i<<" "<<n/i<<endl;

            DFS(result,cur,n/i,i);
            cur.pop_back();
        }
    }
}
vector<vector<int>> getFactors(int n) {
    vvi result;
    vi cur;
    DFS(result,cur,n,2);
    return result;
    // write your code here
}
```

## 46. Permutations
Backtracking with flag

```
class Solution {
public:
    using vi = vector<int>;
    using vvi = vector<vi>;
    using vb = vector<bool>;
    void DFS(vvi&result,vi& nums,vi&cur,vb& visited){
        if(cur.size() == nums.size()){
            result.push_back(cur);
        }
        for(int i=0;i<nums.size();i++){
            if(!visited[i]){
                visited[i] = true;
                cur.push_back(nums[i]);
                DFS(result,nums,cur,visited);
                visited[i] = false;
                cur.pop_back();
            }
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vvi result;
        vi tmp;
        vb visited(nums.size(),false);
        DFS(result,nums,tmp,visited);
        return result;
    }
};
```
<hr>

## 47 Permutations II	
Same trick to prevent duplicated numbers
```
class Solution {
public:
    using vi = vector<int>;
    using vvi = vector<vi>;
    using vb = vector<bool>;
    void DFS(vvi &result, vi&cur,vb&visited,vi&nums){
        if(cur.size() == nums.size()){
            result.push_back(cur);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(!visited[i]){
                visited[i] = true;
                cur.push_back(nums[i]);
                DFS(result,cur,visited,nums);
                cur.pop_back();
                visited[i] = false;

                // only skip same value once we add a number
                while(i+1<nums.size() and nums[i+1] == nums[i]){
                    i++;
                }
            }
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vvi result;
        vi cur;
        vb visited(nums.size(),false);
        DFS(result,cur,visited,nums);
        return result;
    }
};
```

## 31.Next Permutation
#### Iterative
The key is to find a pos in the right that is larger than a pos in the left.

And swap those numebrs and sort the right part(because we change the most significant bit, we
need to make sure that the righter part is the smallest)

```
class Solution {
public:
    // the key for this problem is to find the most significant postion to replace.
    // Find a larger number to replace a smaller number in a more significant bit.
    // Then sort the array
    // if in order, swap
    // if reverse order, find the one the larger than current, swap.
    // and sort(to make sure its small)
    void nextPermutation(vector<int>& nums) {
        if(nums.size()<2){
            reverse(nums.begin(),nums.end());
            return;
        }
        int MaxIndex = nums.size()-1;
        int curIndex = nums.size()-2;
        while(curIndex>=0){
            if(nums[MaxIndex] > nums[curIndex]){
                int targetIndex = MaxIndex;
                for(int i=curIndex+1;i<nums.size();i++){
                    if(nums[i] > nums[curIndex] and nums[i] < nums[MaxIndex]){
                        targetIndex = i;
                    }
                }
                swap(nums[targetIndex],nums[curIndex]);
                sort(nums.begin()+curIndex+1,nums.end());
                return;
            }
            else{
                MaxIndex = curIndex;
                curIndex--;
            }
        }
        reverse(nums.begin(),nums.end());
    }
};
```

#### A simpler implementation
```
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        if(nums.size()<2){
            return ;
        }
        int n = nums.size(), j = n-2;
        // find a postion that is not decreasing
        while(j>=0 and nums[j]>=nums[j+1]) j--;
        if(j<0){
            reverse(nums.begin(),nums.end());
            return ;
        }
        // the part after j+1 must be in decreasing order
        // find the first position that larger than nums[j]
        int i = j+1;
        while(i<n and nums[i]>nums[j]){
            i++;
        }
        //the last number that larger than nums[j] is nums[i-1]
        i--;
        swap(nums[i],nums[j]);
        sort(nums.begin()+j+1,nums.end());
    }
};

```

<hr>

## 60. Permutation Sequence
#### Using next permutation
O(k*n)
```
class Solution {
public:
    void nextPermutation(vector<int>&nums){
        if(nums.size()<2) return;
        int n = nums.size(), j = n-2;
        while(j>=0 and nums[j]>=nums[j+1]) j--;
        if(j<0){
            reverse(nums.begin(),nums.end());
            return;
        }
        int i=j+1;
        while(i<n and nums[i]> nums[j]){
            i++;
        }
        i--;
        swap(nums[i],nums[j]);
        sort(nums.begin()+j+1,nums.end());
    }
    string getPermutation(int n, int k) {
        vector<int> nums(n,0);
        for(int i=1;i<=n;i++){
            nums[i-1] = i;
        }
        for(int i=2;i<=k;i++){
            nextPermutation(nums);
        }
        string result;
        for(int i=0;i<n;i++){
            result+= to_string(nums[i]);
        }
        return result;
    }
};
```

#### Math solution
[O(n)](https://www.cnblogs.com/grandyang/p/4358678.html)

```
class Solution {
public:
    string getPermutation(int n, int k) {
        string num = "123456789";
        vector<int> f(n,1);
        for(int i=1;i<n;i++){
            f[i] = f[i-1]*i;
        }
        //count from 0
        k--;
        string result;
        int pos = n-1;
        while(pos>=0){
            int j = k/f[pos];
            k = k%f[pos];
            result.push_back(num[j]);
            num.erase(num.begin()+j);
            pos--;
        }
        return result;
    }
};
```

## 291 Word Pattern II (Lintcode 829)
Brute Force
```

    bool wordPatternMatch(string &pattern, string &str) {
        // write your code here
        unordered_map<char,string> uMap;
        return helper(pattern,str,0,0,uMap);
    }
    bool helper(string& pattern,string& str,int p,int r,unordered_map<char,string>& uMap){
        if(p == pattern.size() and r == str.size()) return true;
        if(p == pattern.size() or r == str.size()) return false;
        char c = pattern[p];
        for(int i=r;i<str.size();i++){
            string match = str.substr(r,i-r+1);
            if(uMap.count(c) and uMap[c] == match){
                if(helper(pattern,str,p+1,i+1,uMap)) return true;
            }
            // if pattern not exist
            else if(!uMap.count(c)){
                bool b = false;
                for(auto it:uMap){
                    if(it.second == match){
                        b = true;
                    }
                }
                // if b is not a existing pattern
                if(!b){
                    uMap[c] = match;
                    if(helper(pattern,str,p+1,i+1,uMap)) return true;
                    uMap.erase(c);
                }
            }
        }
        return false;
    }
};
```

<hr>

## 140. Word Break II
#### Brute Force DFS: TLE
```
class Solution {
public:
    void DFS(vector<string>&result,vector<string>& cur,string&s,int curIndex,unordered_set<string> uSet){
        if(curIndex == s.size()){
            string tmp;
            for(auto i = 0; i <cur.size();i++){
                tmp += cur[i];
                if(i == cur.size()-1){}
                else tmp += " ";
            }
            result.push_back(tmp);
            return;
        }
        if(curIndex > s.size()) return;
        for(int j=1;j<=s.size()-curIndex;j++){
            string tmp = s.substr(curIndex,j);
            if(uSet.count(tmp)){
                cur.push_back(tmp);
                DFS(result,cur,s,curIndex+j,uSet);
                cur.pop_back();
            }
        }
    }
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        vector<string> result;
        unordered_set<string> uSet;
        vector<string> cur;
        for(auto str:wordDict) uSet.insert(str);
        DFS(result,cur,s,0,uSet);
        return result;
    }
};
```
#### Using Hash map
Using hashmap to record if current string is already visited
```
class Solution {
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        unordered_map<string,vector<string>> uMap;
        return helper(s,wordDict,uMap);
    }
    vector<string> helper(string s,vector<string>&wordDict,unordered_map<string,vector<string>>& uMap){
        if(uMap.count(s)){
            return uMap[s];
        } 
        if (s.empty()) return {""};
        vector<string> res;
        for (string word : wordDict) {
            if (s.substr(0, word.size()) != word) continue;
            vector<string> rem = helper(s.substr(word.size()), wordDict, uMap);
            for (string str : rem) {
                res.push_back(word + (str.empty() ? "" : " ") + str);
            }
        }
        return uMap[s]=res;
    }
};

```
<hr>

## 51. N-Queens
#### Brute force DFS
```
class Solution {
public:
    using vi = vector<int>;
    using vs = vector<string>;
    using vvs = vector<vs>;
    bool check(const vi&positions,int curIndex,int j){
        if(curIndex == 0) return true;
        for(int i=0;i<curIndex;i++){
            //j - positions[i] is the diff between column, curIndex-i is the diff between row
            if((positions[i] == j) or (j-positions[i] == curIndex - i) or (j-positions[i] == i - curIndex)) return false;
        }
        return true;
    }
    void DFS(vvs&result,vi&positions,int curIndex,int n){
        if(curIndex == n){
            vector<string> tmp(n,string(n,'.'));
            for(int i=0;i<positions.size();i++){
                tmp[i][positions[i]] = 'Q';
            }
            result.push_back(tmp);
            return;
        }
        for(int j=0;j<n;j++){
            if(check(positions,curIndex,j)){
                positions[curIndex] = j;
                DFS(result,positions,curIndex+1,n);
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vvs result;
        vi positions(n,0);
        DFS(result,positions,0,n);
        return result;
    }
};
```

<hr>

## 52. N-Queens II


```
class Solution {
public:
    using vi = vector<int>;
    using vs = vector<string>;
    using vvs = vector<vs>;
    bool check(const vi&positions,int curIndex,int j){
        if(curIndex == 0) return true;
        for(int i=0;i<curIndex;i++){
            //j - positions[i] is the diff between column, curIndex-i is the diff between row
            if((positions[i] == j) or (j-positions[i] == curIndex - i) or (j-positions[i] == i - curIndex)) return false;
        }
        return true;
    }
    void DFS(vvs&result,vi&positions,int curIndex,int n){
        if(curIndex == n){
            vector<string> tmp(n,string(n,'.'));
            for(int i=0;i<positions.size();i++){
                tmp[i][positions[i]] = 'Q';
            }
            result.push_back(tmp);
            return;
        }
        for(int j=0;j<n;j++){
            if(check(positions,curIndex,j)){
                positions[curIndex] = j;
                DFS(result,positions,curIndex+1,n);
            }
        }
    }
    int totalNQueens(int n) {
        vvs result;
        vi positions(n,0);
        DFS(result,positions,0,n);
        return result.size();
    }
};
```

<hr>