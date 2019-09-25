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

application layer

persistance layer

<hr>

## 48. Rotate Image
```
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        //inplace: rotate 4 numebrs once a time, not a row once a time;
        int n = matrix.size();
        if(n<= 1) return;
        for(int x=0;x<=n/2;x++){
            for(int y=x;y<n-1-x;y++){
                int tl = matrix[x][y],tr=matrix[y][n-1-x],br=matrix[n-1-x][n-1-y],bl=matrix[n-1-y][x];
                //tl = bl, tr=tl,br=tr
                matrix[x][y] = bl; matrix[y][n-1-x]=tl,matrix[n-1-x][n-1-y]=tr;matrix[n-1-y][x]=br;
            }
        }
    }
};
```

<hr>

## 694 Number of Distinct Islands (Lint 860))
The way to handle duplication is to add all traces first, then normalize the trace.
```
class Solution {
public:
    /**
     * @param grid: a list of lists of integers
     * @return: return an integer, denote the number of distinct islands
     */
     void DFS(vector<vector<int>> &grid,vector<pair<int,int>>& traces,vector<vector<bool>>& visited,int x,int y){
         if(x<0 or x>=grid.size() or y<0 or y>=grid[0].size() or visited[x][y] or grid[x][y]==0) return;
         visited[x][y] = true;
         traces.push_back({x,y});
         DFS(grid,traces,visited,x-1,y);
         DFS(grid,traces,visited,x+1,y);
         DFS(grid,traces,visited,x,y+1);
         DFS(grid,traces,visited,x,y-1);
     }
     string normalize(vector<pair<int,int>> &traces){
         int startX = traces[0].first,startY = traces[0].second;
         string result;
         for(auto &p:traces){
             p.first -= startX;
             p.second -= startY;
         }
         sort(traces.begin(),traces.end());
         for(auto &p:traces){
             result += "["+ to_string(p.first) + "," + to_string(p.second) + "],";
         }
        //  cout<<result<<endl;
         return result;
     }
    int numberofDistinctIslands(vector<vector<int>> &grid) {
        int m = grid.size();
        int n = grid[0].size();
        unordered_set<string> uSet;
        vector<vector<bool>> visited(m,vector<bool>(n,false));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j] and !visited[i][j]){
                    vector<pair<int,int>> traces;
                    DFS(grid,traces,visited,i,j);
                    uSet.insert(normalize(traces));
                }
            }
        }
        
        return uSet.size();
    }
};

```

<hr>

## 711. Number of Distinct Islands II(Lint 804)
DFS的套路不难，这里主要是如何处理小岛的旋转问题。我们的做法是：每次找到一个小岛之后，我们生成其8个对应的变形（包括本身，翻转以及旋转等）。然后返回其排序最小的那个，作为这种类型的小岛的代表。
```
class Solution {
public:
    int numDistinctIslands2(vector<vector<int>>& grid) {
        int row_num = grid.size(), col_num = grid[0].size();
        set<vector<pair<int, int>>> islands;
        for (int r = 0; r < row_num; ++r) {
            for (int c = 0; c < col_num; ++c) {
                if (grid[r][c] == 1) {
                    vector<pair<int, int>> island;
                    DFS(grid, r, c, r, c, island);
                    islands.insert(normalize(island));
                }
            }
        }
        return islands.size();
    }
private:
    void DFS(vector<vector<int>> &grid, int r0, int c0, int r, int c, vector<pair<int, int>> &island) {
        int row_num = grid.size(), col_num = grid[0].size();
        if (r < 0 || r >= row_num || c < 0 || c >= col_num || grid[r][c] <= 0) {
            return;
        }
        grid[r][c] = -1;
        island.push_back(make_pair(r - r0, c - c0));
        for (int d = 0; d < 4; ++d) {
            DFS(grid, r0, c0, r + delta[d][0], c + delta[d][1], island);
        }
    }
    vector<pair<int, int>> normalize(vector<pair<int, int>> &a) {
        vector<vector<pair<int, int>>> ret(8, vector<pair<int, int>>());
        for (auto &p : a) {
            int x = p.first, y = p.second;
            ret[0].push_back(make_pair(x, y));
            ret[1].push_back(make_pair(x, -y));
            ret[2].push_back(make_pair(-x, y));
            ret[3].push_back(make_pair(-x, -y));
            ret[4].push_back(make_pair(y, x));
            ret[5].push_back(make_pair(y, -x));
            ret[6].push_back(make_pair(-y, x));
            ret[7].push_back(make_pair(-y, -x));
        }
        for (int i = 0; i < 8; ++i) {
            //一定要先排序再相减再排序
            sort(ret[i].begin(), ret[i].end());
            int r_offset = 0 - ret[i][0].first, c_offset = 0 - ret[i][0].second;
            for (int j = 0; j < ret[i].size(); ++j) {
                ret[i][j].first += r_offset;
                ret[i][j].second += c_offset;
            }
        }
        sort(ret.begin(), ret.end());
        return ret[0];
    } 
    int delta[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
};
```
<hr>

## 438. Find All Anagrams in a String
Test for all substrings: TLE

```
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> result;
        vector<int> m1(26,0),m2(26,0);
        if(p.size() > s.size()) return result;
        for(auto c:p) m2[c-'a']++;
        for(int i=0;i<p.size();i++){
            m1[s[i]-'a']++;
        }
        if(m1 == m2) result.push_back(0);
        for(int i=p.size();i<s.size();i++){
            m1[s[i]-'a']++;
            m1[s[i-p.size()]-'a']--;
            //don't need to care about if m1[s[i]-'0'] < 0 or not, we will go over all the combinations
            if(m1 == m2) result.push_back(i-p.size()+1);
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
