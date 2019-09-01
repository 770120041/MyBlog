---
layout: post
title:  "Graph-Topological"
categories: Interview
---

## 133. Clone Graph
#### Brute force
Using hash table to set up a coorespondence between origin node and new node. Then DFS again to connect nodes in new graph.
```
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;

    Node() {}

    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/
class Solution {
public:
    Node* cloneNodes(Node* node, unordered_map<Node*,Node*>& uMap){
        if(!node) return NULL;
        Node* curNode = new Node(node->val);
        auto it =uMap.find(node);
        if(it != uMap.end()) return it->second;
        uMap[node] = curNode;
        for(int i=0;i<node->neighbors.size();i++){
            auto it = uMap.find(node->neighbors[i]);
            if(it == uMap.end()){
                 cloneNodes(node->neighbors[i],uMap);
            }
        }
        return curNode;
    }
    void connect(Node* node,Node* curNode,unordered_map<Node*,Node*>& uMap,unordered_set<Node*>& uSet){
        if(!node) return;
        if(uSet.count(node)) return;
        uSet.insert(node);
        curNode->neighbors = vector<Node*>(node->neighbors.size(),NULL);
        for(int i=0;i<node->neighbors.size();i++){
            curNode->neighbors[i] = uMap[node->neighbors[i]];
            connect(node->neighbors[i],curNode->neighbors[i],uMap,uSet);
        }
    }
    Node* cloneGraph(Node* node) {
        unordered_map<Node*,Node*> uMap;
        auto result = cloneNodes(node,uMap);
        unordered_set<Node*> uSet;
        connect(node,result,uMap,uSet);
        return result;    
    }
};
```

<hr>

## 310. Minimum Height Trees
#### Brute force TLE
Calculate size for each node brute force

O(N^2),Because has n nodes and each time needs to go throught the whole graph and take O(N) time.
```
class Solution {
public:
    int calHeight(int curNode,unordered_map<int,vector<int>> &uMap,unordered_set<int> &visited){
        int result = 1;
        visited.insert(curNode);
        auto child = uMap[curNode];
        for(int i=0;i<child.size();i++){
            if(visited.count(child[i]) == 0){
                result = max(calHeight(child[i],uMap,visited)+1,result);
            }
        }
        return result;
    }
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if(n == 1) return {0};
        vector<int> result;
        int resultHeight = INT_MAX;
        unordered_map<int,vector<int>> uMap;
        for(auto edge:edges){
            auto it1 = uMap.find(edge[0]);
            if(it1==uMap.end()){
                vector<int> tmp{edge[1]};
                uMap[edge[0]] = tmp;
            }
            else it1->second.push_back(edge[1]);
            auto it2 = uMap.find(edge[1]);
            if(it2==uMap.end()){
                vector<int> tmp{edge[0]};
                uMap[edge[1]] = tmp;
            }
            else it2->second.push_back(edge[0]);
        }
        for(auto it = uMap.begin();it!=uMap.end();it++){
            unordered_set<int> visited;
            int curNode = it->first;
            int height = calHeight(curNode,uMap,visited);
            if(height <= resultHeight){
                if(height == resultHeight) {}
                else{
                    result.clear(); 
                    resultHeight = height;
                }
                result.push_back(curNode);
            }
        }
        return result;
    }
};
```

#### 拓扑排序，剥洋葱
这道题虽然是树的题目，但是跟其最接近的题目是Course Schedule 课程清单和Course Schedule II 课程清单之二。发现大家推崇的方法是一个类似剥洋葱的方法，就是一层一层的褪去叶节点，最后剩下的一个或两个节点就是我们要求的最小高度树的根节点。所有入度（即相连边数）为 1 的节点即是叶子节点，如果一个节点要是根，那么这样最后总能找到一个根。如何证明是最小的节点？

最多只能有两个结果，原因是因为图中没有环，所以只能有一个或者两个节点。

因为要一层一层剥除，自然想到用queue来做层序遍历。
```
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if(n == 1) return {0};
        vector<int> result;
        vector<int> degree(n,0);
        vector<vector<bool>> matrix(n,vector<bool>(n,false));
        for(auto edge:edges){
            degree[edge[0]]++, degree[edge[1]]++;
            matrix[edge[0]][edge[1]] = matrix[edge[1]][edge[0]] = true;
        }
        queue<int> q;
        for(int i=0;i<n;i++){
            if(degree[i] == 1){
                degree[i]--;
                q.push(i);
            }
        };
        while(!q.empty()){
            int size = q.size();
            result.clear();
            for(int i=0;i<size;i++){
                int cur = q.front();q.pop();
                result.push_back(cur);
                for(int j=0;j<n;j++){
                    if(matrix[cur][j]){
                        degree[j]--;
                        if(degree[j] == 1) q.push(j);
                    }
                }
            }
        }
        return result;
    }
};
```

#### using hash map to speed up
O(N)
```
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if(n == 1) return {0};
        vector<int> result;
        vector<int> degree(n,0);
        unordered_map<int,vector<int>> uMap;
        for(auto edge:edges){
            degree[edge[0]]++, degree[edge[1]]++;
            auto it1 = uMap.find(edge[0]);
            if(it1==uMap.end()){
                vector<int> tmp{edge[1]};
                uMap[edge[0]] = tmp;
            }
            else it1->second.push_back(edge[1]);
            auto it2 = uMap.find(edge[1]);
            if(it2==uMap.end()){
                vector<int> tmp{edge[0]};
                uMap[edge[1]] = tmp;
            }
            else it2->second.push_back(edge[0]);
        }
        
        queue<int> q;
        for(int i=0;i<n;i++){
            if(degree[i] == 1){
                degree[i]--;
                q.push(i);
            }
        };
        while(!q.empty()){
            int size = q.size();
            result.clear();
            for(int i=0;i<size;i++){
                int cur = q.front();q.pop();
                result.push_back(cur);
                auto vec = uMap[cur];
                for(int j=0;j<vec.size();j++){
                    if(degree[vec[j]] != 0){
                        degree[vec[j]]--;
                        if(degree[vec[j]] == 1) q.push(vec[j]);
                    }
                }
            }
        }
        return result;
    }
};
```
<hr>

## 207. Course Schedule
#### Topo sort
```
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        if(numCourses<2) return true;
        vector<int> inDegree(numCourses,0);
        vector<vector<bool>> matrix(numCourses,vector<bool>(numCourses,false));
        for(auto edge:prerequisites){
            inDegree[edge[0]]++;
            matrix[edge[1]][edge[0]] = true;
        }
        queue<int> q;
        for(auto j=0;j<numCourses;j++){
            if(inDegree[j] == 0) q.push(j);
        }
        while(!q.empty()){
            int cur = q.front(); q.pop();
            for(int j=0;j<numCourses;j++){
                if(matrix[cur][j]){
                    inDegree[j]--;
                    if(inDegree[j] == 0) q.push(j);
                }
            }
        }
        for(auto x:inDegree){
            if(x != 0) return false;
        }
        return true;
    }
};
```

<hr>

## 210. Course Schedule II
If not exist return {}
```
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        if(numCourses == 0) return {};
        if(numCourses == 1) return {0};
        vector<int> inDegree(numCourses,0);
        vector<vector<bool>> matrix(numCourses,vector<bool>(numCourses,false));
        for(auto edge:prerequisites){
            inDegree[edge[0]]++;
            matrix[edge[1]][edge[0]] = true;
        }
        vector<int> result;
        queue<int> q;
        for(auto j=0;j<numCourses;j++){
            if(inDegree[j] == 0) q.push(j);
        }
        while(!q.empty()){
            int cur = q.front(); q.pop();
            result.push_back(cur);
            for(int j=0;j<numCourses;j++){
                if(matrix[cur][j]){
                    inDegree[j]--;
                    if(inDegree[j] == 0) q.push(j);
                }
            }
        }
        for(auto j=0;j<numCourses;j++){
            if(inDegree[j] != 0) return {};
        }
        return result;
    }
};
```

<hr>

## 269 Alien Dictionary (Lint 892)
[blog](https://www.cnblogs.com/grandyang/p/5250200.html)
```
class Solution {
public:
    /**
     * @param words: a list of words
     * @return: a string which is correct order
     */
    string alienOrder(vector<string>& words) {
        set<pair<char, char>> st;
        unordered_set<char> ch;
        vector<int> in(256);
        queue<char> q;
        string res;
        for (auto a : words) ch.insert(a.begin(), a.end());
        for (int i = 0; i < (int)words.size() - 1; ++i) {
            int mn = min(words[i].size(), words[i + 1].size()), j = 0;
            for (; j < mn; ++j) {
                if (words[i][j] != words[i + 1][j]) {
                    st.insert(make_pair(words[i][j], words[i + 1][j]));
                    break;
                }
            }
            if (j == mn && words[i].size() > words[i + 1].size()) return "";
        }
        for (auto a : st) ++in[a.second];
        for (auto a : ch) {
            if (in[a] == 0) {
                q.push(a);
                res += a;
            } 
        }
        while (!q.empty()) {
            char c = q.front(); q.pop();
            for (auto a : st) {
                if (a.first == c) {
                    --in[a.second];
                    if (in[a.second] == 0) {
                        q.push(a.second);
                        res += a.second;
                    }
                }
            }
        }
        return res.size() == ch.size() ? res : "";
    }
};
```

<hr>