---
layout: post
title:  "Leetcode DFS BFS "
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

<hr>

## 339 Nested List Weight Sum	
Simple DFS
```
class Solution {
public:
    int depthSum(const vector<NestedInteger>& nestedList) {
        int result = 0;
        for(auto nested:nestedList){
            DFS(result,nested,1);
        }
        return result;
    }
    void DFS(int &result,NestedInteger cur,int depth){
        if(cur.isInteger()){
            result += cur.getInteger()*depth;
            return;
        }
        for(auto child: cur.getList()){
            DFS(result,child,depth+1);
        }
    }
};
```

<hr>

## 364 Nested List Weight Sum II	
DFS one pass, the reason why there is replicate code is that we don't have the constructor of NestedInteger, otherwise we can call the DFS directly.

The idea is cal the depth and cal the sum at the same time. When we reach the leaft node, we cal the the result at depth 1, and then for none leaf nodes, we cal the height by firstly go through all none interger nodes then cal the result of interger nodes.

#### Careful
Be careful the the weight of each leaf is not 1, so would be wrong
```
int depthSumInverse(vector<NestedInteger> nestedList) {
    int result = 0;
    DFS(result,nestedList);
    return result;
}

//所有leaves 的level是1是不成的，因为倒着推上去，上一级的深度会冲突。刚开始我也想是不是每个leaf 都是1,最后发现那样的话就没法做题了
int DFS(int& result,vector<NestedInteger> nestedList){
    if(nestedList.size() == 0) return 1;
    int depth = 1;
    int curNumbers = 0;
    for(auto child:nestedList){
        if(child.isInteger()){
            curNumbers += child.getInteger();
        }
        else{
            depth = max(depth,1+DFS(result,child.getList()));
        }
    }
    result += (curNumbers)*depth;
    cout<<"d:"<<depth<<",nums:"<<curNumbers<<endl;
    return depth;
}
```
#### DFS solution:
get depth first then dfs
```py
class Solution:
    # when we meet [1,7,[7,[9]],[12],[],[]], the height of [12] is actually 2, not 1, another way is we dfs the height first, then calculate
    def depthSumInverse(self, nestedList):
        self.sum = 0
        def getDepth(nl):
            h = 1
            for ni in nl:
                if ni.isInteger(): pass
                else: h = max(h,1+getDepth(ni.getList()))
            return h
        def dfs(nls,h):
            cur = 0
            for ni in nls:
                if ni.isInteger():
                    cur += ni.getInteger()
                else:
                    dfs(ni.getList(),h-1)
            self.sum += cur * h
        h = getDepth(nestedList)
        dfs(nestedList,h)
        return self.sum
```

#### BFS Level order traversal
Using weighted and unweighed, so the that each layer can be added to weighted by depth times.

Remeber to add interger first then go to its child, this makes it possible to add depth times cur integer.

**Here remeber to clean the queue every time before add to weighted! This is a way to go throguh all nodes with same layer!!**
```
class Solution {
public:
    int depthSumInverse(vector<NestedInteger>& nestedList) {
        int unweighted = 0, weighted = 0;
        queue<vector<NestedInteger>> q;
        q.push(nestedList);
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                vector<NestedInteger> t = q.front(); q.pop();
                for (auto a : t) {
                    if (a.isInteger()) unweighted += a.getInteger();
                    else if (!a.getList().empty()) q.push(a.getList());
                }
            }
            weighted += unweighted;
        }
        return weighted;
    }
};
```


## 752. Open the Lock
DFS not working, because find the neares is too slow. (using visited to record, if the next search is slower then DFS this pos, but this may be still O(K^N))

BFS
```
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        unordered_set<string> visited;
        // unordered_set<string> deadendsSet;
        for(auto s: deadends){
            visited.insert(s);
        }
        if(visited.count(target) || visited.count("0000")) return -1;
        char coor[] = {'9','0','1','2','3','4','5','6','7','8','9','0'};
        queue<string> mQueue;
        queue<int> cQueue;
        mQueue.push("0000");
        cQueue.push(0);
        visited.insert("0000");
        while(!mQueue.empty()){
            auto curString = mQueue.front();
            mQueue.pop();
            auto curCount = cQueue.front();
            cQueue.pop();
            // cout<<curString <<" "<<curCount<<endl;
            if(curString == target){
                return curCount;
            }
            for(int i=0;i<4;i++){
                auto next = curString;
                next[i] = coor[curString[i]-'0'];
                if(!visited.count(next)){
                    visited.insert(next);
                    mQueue.push(next);
                    cQueue.push(curCount+1);
                }
                next[i] = coor[curString[i]-'0'+2];
                if(!visited.count(next)){
                    visited.insert(next);
                    mQueue.push(next);
                    cQueue.push(curCount+1);
                }
            }
        }
        return -1;
    }
};
```

## 127. Word Ladder
Crap 

This problem is bad because `start='a',end='c',list=[a,b,c]` result in 2.

So a->a is also counted as one transform.

```
class Solution {
public:
    bool check(string a,string b){
        int cnt = 0;
        for(int i=0;i<a.size();i++){
            if(a[i] != b[i]) cnt++;
            if(cnt == 2) return false;
        }
        return true;
    }
    typedef struct{
        string str;
        int cnt;
    }Node;
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        queue<Node> myQueue;
        int n=wordList.size();
        myQueue.push(Node{beginWord,1});
        vector<bool> visited(n,false);
        while(!myQueue.empty()){
            Node cur = myQueue.front();
            myQueue.pop();
            
            if(cur.str == endWord) return cur.cnt;
            for(int i=0;i<n;i++){
                if(!visited[i] && check(cur.str,wordList[i])){
                    visited[i] = true;
                    myQueue.push(Node{wordList[i],cur.cnt+1});
                }
            }
        }
        return 0;
    }
};
```

## 126. Word Ladder II
Brute force BFS with recording all neighbors: TLE
```
class Solution {
public:
    typedef struct{
        string str;
        int dis;
        vector<string> list;
    }Node;
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        int n = wordList.size();
        unordered_set<string> visited;
        vector<vector<string>> result;
        unordered_map<string,unordered_set<string>> neighbors;
        
        // enumerate all neighbors
        vector<string> addToNeighbor(wordList);
        addToNeighbor.push_back(beginWord);
        for(int i=0;i<addToNeighbor.size();i++){
            unordered_set<string> tmpNeighbor;
            for(int j=0;j<beginWord.size();j++){
                string tmpString = addToNeighbor[i];
                for(int k=0;k<26;k++){
                    tmpString[j] = 'a'+k;
                    tmpNeighbor.insert(tmpString);
                }
            }
            neighbors[addToNeighbor[i]] = tmpNeighbor;
        }
        
        // vector<bool> visited(n,false);
        queue<Node> myQueue;
        vector<string> init;
        init.push_back(beginWord);
        myQueue.push( Node{beginWord,0,init});
        while(!myQueue.empty()){
            int size = myQueue.size();
            for(int m=0;m<size;m++){
                Node cur = myQueue.front(); myQueue.pop();
                visited.insert(cur.str);
                if(cur.str == endWord){
                    result.push_back(cur.list);
                    continue;
                }
                vector<string> nextList(cur.list);
                for(int i=0;i<n;i++){
                    if(visited.count(wordList[i]) == 0 and neighbors[cur.str].count(wordList[i]) == 1){
                        nextList.push_back(wordList[i]);
                        myQueue.push(Node{wordList[i],cur.dis+1,nextList});
                        nextList.pop_back();
                    }
                }
            }
            if(!result.empty()) break;
        }
        return result;
    }
};
```

## 78. Subsets
No repeat, so Index increase monotaneously. When curIndex reaches n, its done.
```
void DFS(vector<vector<int>>&result,vector<int> &nums,vector<int> cur ,int curIndex,int n){
    result.push_back(cur);
    if(curIndex == n) return;
    for(int i=curIndex;i<n;i++){
        cur.push_back(nums[i]);
        DFS(result,nums,cur,i+1,n);
        cur.pop_back();
    }
}
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> empty;
    DFS(result,nums,empty,0,nums.size());
    return result;        
}
```

## 90. Subsets II
The key to this is: 如果不是这个DFS的第一个数而且上一个数和这一个相同，那么跳过。原因是：每一次递归都给当前的vector增加了一个数字，但如果两个数字相同，那么比如`[1,2,2]`,就会把`[1,2],[1,2]`一起加入，这是不对的。加一个if就可以避免这种情况。
```
void DFS(vector<vector<int>>&result, vector<int> &nums,vector<int> cur,int curIndex, int n){
    result.push_back(cur);
    if(curIndex == n){
        return;
    }
    for(int i=curIndex;i<n;i++){
        // duplicates
        if(i>curIndex && nums[i-1] == nums[i]){
            continue;
        }
        cur.push_back(nums[i]);
        DFS(result,nums,cur,i+1,n);
        cur.pop_back();
    }
    
}
vector<vector<int>> subsetsWithDup(vector<int>& nums) {
    sort(nums.begin(),nums.end());
    vector<int> empty;
    vector<vector<int>> result;
    DFS(result,nums,empty,0,nums.size());
    return result;
}
```

## 22 Generating Parenthesis
```
// how many left used
void DFS(vector<string>&result,string cur,int left,int right,int n){
    if(left+right == 2*n){
        result.push_back(cur);
        return;
    }
    if(left < n){
        DFS(result,cur+"(",left+1,right,n);
    }
    if(right < left){
        DFS(result,cur+")",left,right+1,n);
    }
}
vector<string> generateParenthesis(int n) {
    vector<string> result;
    string cur;
    DFS(result,"",0,0,n);
    return result;
}
```