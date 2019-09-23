---
layout: post
title:  Amazon Tags
categories: Interview
---

## 763. Partition Labels
Same as Merge Intervels
```
class Solution {
public:
    static bool cmp(vector<int> a, vector<int> b){
        return a[0]<b[0] or (a[0] == b[0] and a[1] < b[1]);
    }
    vector<int> partitionLabels(string S) {
        unordered_map<char,vector<int>> uMap;
        for(auto i = 0;i<S.size();i++){
            auto it = uMap.find(S[i]);
            if(it == uMap.end()){
                uMap[S[i]] = {i,i};
            }
            else{
                it->second[1] = i;
            }
        }
        vector<vector<int>> intervals;
        for(auto it=uMap.begin();it!=uMap.end();it++) intervals.push_back(it->second);
        
        //merge intervals from here
        sort(intervals.begin(),intervals.end(),cmp);
        int validSize = 0;
        for(auto i=1;i<intervals.size();i++){
            if(intervals[i][0] < intervals[validSize][1]){
                intervals[validSize][1] = max(intervals[i][1],intervals[validSize][1]);
            }
            else intervals[++validSize] = intervals[i];
        }
        vector<int> result;
        for(int i=0;i<=validSize;i++){
            result.push_back(intervals[i][1]-intervals[i][0]+1);
        }
        return result;
    }
};
```
<hr>

## 675. Cut Off Trees for Golf Event
Brute force, BFS for each node

Can use floyid algorithm to speed up?
```
class Solution {
public:
    static bool cmp(vector<int> &a,vector<int> &b){
        return a[0] < b[0];
    }
    int helper(vector<vector<int>>& forest, int row, int col, int treeRow, int treeCol) {
        if (row == treeRow && col == treeCol) return 0;
        int m = forest.size(), n = forest[0].size(), cnt = 0;
        queue<int> q{{row * n + col}};
        vector<vector<int>> visited(m, vector<int>(n));
        vector<int> dir{-1, 0, 1, 0, -1};
        while (!q.empty()) {
            ++cnt;
            for (int i = q.size(); i > 0; --i) {
                int r = q.front() / n, c = q.front() % n; q.pop();
                for (int k = 0; k < 4; ++k) {
                    int x = r + dir[k], y = c + dir[k + 1];
                    if (x < 0 || x >= m || y < 0 || y >= n || visited[x][y] == 1 || forest[x][y] == 0) continue;
                    if (x == treeRow && y == treeCol) return cnt;
                    visited[x][y] = 1;
                    q.push(x * n + y);
                }
            }
        }
        return -1;
    }
    int cutOffTree(vector<vector<int>>& forest) {
        vector<vector<int>> trees;
        int m = forest.size();
        int n = forest[0].size();
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(forest[i][j] > 1){  
                    vector<int> tmp{forest[i][j],i,j};
                    trees.push_back(tmp);
                }
            }
        }
        sort(trees.begin(),trees.end(),cmp);
        vector<int> curPos{0,0};
        int result = 0;
        for(int i=0;i<trees.size();i++){
            // cout<<"dest="<<trees[i][1]<<","<<trees[i][2]<<endl;
            // cout<<"start="<<curPos[0]<<","<<curPos[1]<<endl;
            vector<int> dst{trees[i][1],trees[i][2]};
            int dis = helper(forest,curPos[0],curPos[1],dst[0],dst[1]);
            if(dis == -1) return -1;
            result += dis;
            curPos = {trees[i][1],trees[i][2]};
        }
        return result;
    }
};
```
<hr>