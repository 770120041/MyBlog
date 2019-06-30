---
layout: post
title:  "Leetcode BFS"
categories: Interview
---

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


## 286	Walls and Gates
#### DFS
```
void DFS(vector<vector<int>> &rooms,int i,int j,int m,int n, int dis){
        if(i<0 or i>= m or j<0 or j>=n or rooms[i][j] < dis){
            return;
        }
        rooms[i][j] = dis;
        DFS(rooms,i-1,j,m,n,dis+1);
        DFS(rooms,i+1,j,m,n,dis+1);
        DFS(rooms,i,j-1,m,n,dis+1);
        DFS(rooms,i,j+1,m,n,dis+1);
    } 
void wallsAndGates(vector<vector<int>> &rooms) {
    // write your code here
    int m = rooms.size();
    if(m==0) return;
    int n = rooms[0].size();
    if(n==0) return ;
    for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
            if(rooms[i][j] == 0){
                DFS(rooms,i,j, m ,n ,0);
            }
        }
    }
}
```

#### BFS simple solution.
```
typedef struct{
    int x;
    int y;
    int dis;
}Point;
void wallsAndGates(vector<vector<int>> &rooms) {
    // write your code here
    int m = rooms.size();
    int n = rooms[0].size();
    for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
            if(rooms[i][j] == 0){
                queue<Point> myQueue;
                myQueue.push(Point{i,j,0});
                while(!myQueue.empty()){
                    Point cur = myQueue.front();
                    myQueue.pop();
                    if(cur.x < 0 or cur.x >= m or cur.y < 0 or cur.y >= n or rooms[cur.x][cur.y] < cur.dis){
                        continue;
                    }
                    rooms[cur.x][cur.y] = cur.dis;
                    myQueue.push(Point{cur.x,cur.y+1,cur.dis+1});
                    myQueue.push(Point{cur.x,cur.y-1,cur.dis+1});
                    myQueue.push(Point{cur.x+1,cur.y,cur.dis+1});
                    myQueue.push(Point{cur.x-1,cur.y,cur.dis+1});
                }
            }
        }
    }
}
```