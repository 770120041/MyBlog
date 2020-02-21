---
layout: post
title:  "Leetcode UnionFind-Trie"
categories: Interview
---
# 并查集 Union Find
在计算机科学中，并查集是一种树型的数据结构，用于处理一些不交集（Disjoint Sets）的合并及查询问题。有一个联合-查找算法（union-find algorithm）定义了两个用于此数据结构的操作：

Find：确定元素属于哪一个子集。它可以被用来确定两个元素是否属于同一子集。
Union：将两个子集合并成同一个集合。
由于支持这两种操作，一个不相交集也常被称为联合-查找数据结构（union-find data structure）或合并-查找集合（merge-find set）。其他的重要方法，MakeSet，用于创建单元素集合。有了这些方法，许多经典的划分问题可以被解决。

为了更加精确的定义这些方法，需要定义如何表示集合。一种常用的策略是为每个集合选定一个固定的元素，称为代表，以表示整个集合。接着，Find(x) 返回 x 所属集合的代表，而 Union 使用两个集合的代表作为参数。

[blog](https://blog.csdn.net/Yaokai_AssultMaster/article/details/78888846)
```
public class QuickFind {
public: 
    bool connected(int q,int p){
        return find(q) == find(p);
    }
    int find(int q){
        while(pre[q] != q){
            pre[q] = find(pre[q]);
        }
        return pre[q];
    }
    void union(int p,int q){
        int rootp = find(p);
        int rootq = find(q);
        if(rootp != rootq) pre[rootp] = pre[rootq];
    }
}
```

#### 547. Friend Circles
Template question
```
class Solution {
public:
    int find(vector<int>&pre,int p){
        if(pre[p] != p){
            p = find(pre,pre[p]);
        }
        return pre[p];
    }
    
    void unionFunction(vector<int>&pre,int p,int q){
        int rootP = find(pre,p);
        int rootQ = find(pre,q);
        pre[rootP] = rootQ;
    }
    
    int findCircleNum(vector<vector<int>>& M) {
        int n = M.size();
        unordered_set<int> uSet;
        vector<int> pre(n,0);
        for(auto i=0;i<n;i++) pre[i] = i;
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(M[i][j]) unionFunction(pre,i,j);
            }
        }
        for(auto x : pre){
            uSet.insert(find(pre,x));
        }
        return uSet.size();
    }
};
```

<hr>

## 684. Redundant Connection
Template question,注意这里是
```
int find(vector<int>&pre,int p){
        if(pre[p] != p){
            pre[p] = find(pre,pre[p]);
        }
        return pre[p];
    }
    
```
#### Union find
```
class Solution {
public:
    //最小生成树，但每条边都是1
    int find(vector<int>&pre,int p){
        if(pre[p] != p){
            pre[p] = find(pre,pre[p]);
        }
        return pre[p];
    }
    
    void unionF(vector<int>&pre,int p,int q){
        int rootP = find(pre,p);
        int rootQ = find(pre,q);
        pre[rootP] = rootQ;
    } 
    
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        vector<int> result;
        vector<int> pre(1005,0);
        for(auto i=0;i<1005;i++) pre[i] = i;
        for(auto x:edges){
            if(find(pre,x[0]) == find(pre,x[1])){
                result = x;
                break;
            }
            else{
                unionF(pre,x[0],x[1]);
            }
        }
        return result;
    }
};
```

<hr>

## 685. Redundant Connection II
If use Union find again, `[[2,1],[3,1],[4,2],[1,4]]` can't pass

[blog](https://www.cnblogs.com/grandyang/p/8445733.html)
[leetcode discussion](https://leetcode.com/problems/redundant-connection-ii/discuss/278105/topic)
同时检查入度和环，入度用一个数组记录，环用并查集记录
1. 只有入度为2：删除第二个增加入度的边
2. 只有环没有入度2：删除导致环的点
3. 有入度和环：删除导致且使得入度为2的点（另一个边不会导致环）
总结：导致环或者入度为2，则删除是不行的， 因为`[[2,1],[3,1],[4,2],[1,4]]`这种状态3的情况，如果删除第二个出现的`3,1`，会导致顶点数目不够，所以要删除即构成环又导致入度为2的点。

#### Union find
```
class Solution {
public:
    //最小生成树，但每条边都是1
    int getRoot(vector<int>&pre,int p){
        if(pre[p] != p){
            pre[p] = getRoot(pre,pre[p]);
        }
        return pre[p];
    }
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        vector<int> root(1005,0),first,second;
        //root is used as degree
        for(auto &x : edges){
            if(root[x[1]] == 0){
                root[x[1]] = x[0];
            }
            else{
                first = {root[x[1]],x[1]};
                second = x;
                x[1] = 0; //skip the checking of edge2, which is x[1]
            }
        }
        //reset root
        for(int i=0;i<1005;i++) root[i] = i;
        for(auto edge:edges){
            if(edge[1] == 0) continue;
            int x = getRoot(root, edge[0]), y = getRoot(root, edge[1]);
            if (x == y) return first.empty() ? edge : first;
            root[x] = y;
        }
        return second;
    }
};
```

<hr>

## 261 Graph Valid Tree (Lint 178)

```
class Solution {
public:
    int findRoot(vector<int>&pre,int cur){
        if(pre[cur] != cur){
            pre[cur] = findRoot(pre,pre[cur]);
        } 
        return pre[cur];
    }
    bool validTree(int n, vector<vector<int>> &edges) {
        if(n <= 1) return true;
        vector<int> pre(n,0);
        for(int i=0;i<n;i++) pre[i] = i;
        unordered_set<int> visited;
        for(auto edge:edges){
            visited.insert(edge[0]); visited.insert(edge[1]);
            int rootA = findRoot(pre,edge[0]);
            int rootB = findRoot(pre,edge[1]);
            if(rootA == rootB) return false;
            pre[rootA] = rootB;
        }
        return visited.size() == n;
    }
};
```

<hr>

## 305. Number of Islands II(Lint 434)
#### 2D Union Find
```

class Solution {
public:
    using PPoint = pair<int,int>; 
    PPoint findRoot(PPoint a,map<PPoint,PPoint> &pre,int &rootNum){
        auto it = pre.find(a);
        if(it == pre.end()) {
            rootNum++;
            return pre[a] = a;
        }
        if(pre[a].first != a.first or pre[a].second != a.second){
            pre[a] = findRoot(pre[a],pre,rootNum);
        }
        return pre[a];
    }
    void unionF(PPoint a,PPoint b,map<PPoint,PPoint> &pre,int &rootNum,vector<vector<int>>&matrix,int n,int m){
        int bx = b.first,by=b.second;
        auto rootA = findRoot(a,pre,rootNum);
        if(bx < 0 or bx >= n or by<0 or by >= m or matrix[bx][by] == 0) return;
        auto rootB = findRoot(b,pre,rootNum);
        if(rootA.first == rootB.first and rootA.second == rootB.second) return;
        pre[rootA] = rootB;
        rootNum--;
    }
    vector<int> numIslands2(int n, int m, vector<Point> &operators) {
        vector<int> result;
        int rootNum=0;
        vector<vector<int>> matrix(n,vector<int>(m,0));
        map<PPoint,PPoint> pre;
        for(int i=0;i<operators.size();i++){
            int x = operators[i].x, y = operators[i].y;
            matrix[x][y] = 1;
            unionF({x,y},{x+1,y},pre,rootNum,matrix,n,m);
            unionF({x,y},{x-1,y},pre,rootNum,matrix,n,m);
            unionF({x,y},{x,y-1},pre,rootNum,matrix,n,m);
            unionF({x,y},{x,y+1},pre,rootNum,matrix,n,m);
            result.push_back(rootNum);
        }
        return result;
    }
};
```
<hr>

## 323 Number of Connected Components in an Undirected Graph
Easy union find
```
class Solution {
public:
    int countComponents(int n, vector<pair<int, int> >& edges) {
        vector<int> pre(n,0);
        for(int i=0;i<n;i++) pre[i] = i;
        int result = n;
        for(auto edge:edges){
            int rootA = findPre(pre,edge[0]);
            int rootB = findPre(pre,edge[1]);
            if(rootA == rootB) continue;
            result--;
            pre[rootA] = rootB;
        }
        return result;
    }
    int find(vector<int> &pre, int i) {
        if(pre[i]!=i){
            pre[i] = find(pre,pre[i]);
        }
        return pre[i];
    }
};
```
<hr>

# Trie
## 211 Add and Search Word - Data structure design
```
class WordDictionary {
    
public:
    struct TrieNode {
    public:
        TrieNode *child[26];
        bool isWord;
        TrieNode() : isWord(false) {
            for (auto &a : child) a = NULL;
        }
    };
    /** Initialize your data structure here. */
    TrieNode* Root;
    WordDictionary() {
        Root = new TrieNode();
    }
    
    /** Adds a word into the data structure. */
    void addWord(string word) {
        TrieNode* p = Root;
        for (auto &a : word) {
            int i = a - 'a';
            if (!p->child[i]) p->child[i] = new TrieNode();
            p = p->child[i];
        }
        p->isWord = true;
    }
    
    bool search(string word) {
        return searchWord(word, Root, 0);
    }
    
    bool searchWord(string &word, TrieNode *p, int i) {
        if (i == word.size()) return p->isWord;
        if (word[i] == '.') {
            for (auto &a : p->child) {
                if (a && searchWord(word, a, i + 1)) return true;
            }
            return false;
        } else {
            return p->child[word[i] - 'a'] && searchWord(word, p->child[word[i] - 'a'], i + 1);
        }
    }
};

```
<hr>

## 208 Implement Trie (Prefix Tree)
```
class Trie {
struct Node{
    Node* child[26];
    bool isString;
    Node():isString(false){
        for(auto &a:child) a = NULL;
    }
};
public:
    
    /** Initialize your data structure here. */
    Node* Root;
    Trie() {
        Root = new Node();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        Node* cur = Root;
        for(auto &a:word){
            int pos = a - 'a';
            if(!cur->child[pos]) cur->child[pos] = new Node();
            cur = cur->child[pos];
        }
        cur->isString = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        stack<pair<Node*,int>> s;
        s.push({Root,0});
        while(!s.empty()){
            auto cur = s.top(); s.pop();
            if(cur.second == word.size()){
                if(cur.first->isString) return true;
                continue;
            }
            if(word[cur.second] == '.'){
                for(int i=0;i<26;i++){
                    if(cur.first->child[i]){
                        s.push({cur.first->child[i],cur.second+1});
                    }
                }
            }
            else{
                if(cur.first->child[word[cur.second]-'a']){
                    s.push({cur.first->child[word[cur.second]-'a'],cur.second+1});
                }
            }
        }
        return false;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string word) {
        stack<pair<Node*,int>> s;
        s.push({Root,0});
        while(!s.empty()){
            auto cur = s.top(); s.pop();
            if(cur.second == word.size()){
                return true;
            }
            if(word[cur.second] == '.'){
                for(int i=0;i<26;i++){
                    if(cur.first->child[i]){
                        s.push({cur.first->child[i],cur.second+1});
                    }
                }
            }
            else{
                if(cur.first->child[word[cur.second]-'a']){
                    s.push({cur.first->child[word[cur.second]-'a'],cur.second+1});
                }
            }
        }
        return false;
    }
};

```
<hr>

## 212 Word Search II
这道题本质上还是搜索字符串存在，如果只需要查找某一个字符串是否存在,那么暴力搜就是最简单的办法。但如果对每个字符串都用DFS找，缺点就是上次查找的结果不能被利用，所以想要使用Trie Tree来帮助存储之前搜索的结果。注意不能用DP+memo，原因是Trie树能保存相同的前缀，但DP with memo不知道是不是有相同的前缀。使用Trie能够尽量利用前缀的特点，尽量少的DFS。
<!-- 
```cpp
class Solution {
public:
    struct TrieNode{
        string str;
        TrieNode* child[26];
        TrieNode(){
            for(auto &a:child) a = NULL;
        }
    };
    struct TrieTree{
        TrieNode* root;
        TrieTree(){
            root = new TrieNode();
        }
        void insert(string s) {
            TrieNode *p = root;
            for (auto &a : s) {
                int i = a - 'a';
                if (!p->child[i]) p->child[i] = new TrieNode();
                p = p->child[i];
            }
            p->str = s;
        }
    };
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        TrieTree* t = new TrieTree(); 
        vector<string> result;
        if(board.empty() or words.empty()) return result;
        int m = board.size();
        int n = board[0].size();
        vector<vector<bool>> visited(m,vector<bool>(n,false));
        for(auto  &s:words) t->insert(s);
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[0].size();j++){
                char cur = board[i][j];
                if(t->root->child[cur-'a'] ){
                    search(board,t->root->child[cur-'a'],i,j,visited,result);
                }
            }
        }
        return result;
    }
    
    void search(vector<vector<char>>& board, TrieNode* p, int i, int j, vector<vector<bool>>& visit, vector<string>& res) { 
        if (!p->str.empty()) {
            res.push_back(p->str);
            p->str.clear();
        }
        int d[][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        visit[i][j] = true;
        for (auto &a : d) {
            int nx = a[0] + i, ny = a[1] + j;
            if (nx >= 0 && nx < board.size() && ny >= 0 && ny < board[0].size() && !visit[nx][ny] && p->child[board[nx][ny] - 'a']) {
                search(board, p->child[board[nx][ny] - 'a'], nx, ny, visit, res);
            }
        }
        visit[i][j] = false;
    }
};
``` -->

<hr>