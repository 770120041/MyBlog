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