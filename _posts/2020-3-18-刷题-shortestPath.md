---
layout: post
title:  Python Shortest path
categories: Interview
---

## 1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance
Floyd algrithm
```py
class Solution(object):
    def findTheCity(self, n, edges, distanceThreshold):
        # floyd alogrithm
        # initially i to j is +inif, if there is an edge, then it is edge[i][j]
        
        def compare(a,b):
            if a[1] < b[1]:
                return -1
            elif a[1] > b[1]:
                return 1
            if a[0] > b[0]:
                return -1
            return 1
        dis = [[999999999 for i in range(n)] for j in range(n)]
        for edge in edges:
            dis[edge[0]][edge[1]] = edge[2]
            dis[edge[1]][edge[0]] = edge[2]
        for k in range(n):
            for i in range(n):
                for j in range(n):
                    dis[i][j] = min(dis[i][j], dis[i][k]+dis[k][j])
        t = []
        for i in range(n):
            cnt = 0
            for j in range(n):
                if j != i and dis[i][j] <= distanceThreshold:
                    cnt += 1
            t.append((i,cnt))
        t.sort(cmp=compare)
        return t[0][0]
```
## 1401. Circle and Rectangle Overlapping
Find the shortest distance from the center to the rectangle, return dist <= radius.

```cpp
class Solution {
public:
    bool checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2) {
        int xclose,yclose;
        if(x_center<x1) xclose=x1;
        else if(x_center>x2) xclose=x2;
        else xclose=x_center;
        
        if(y_center<y1) yclose=y1;
        else if(y_center>y2) yclose=y2;
        else yclose=y_center;
        
        return (x_center-xclose)*(x_center-xclose)+(y_center-yclose)*(y_center-yclose)<=radius*radius;
    }
};
```

## 1402. Reducing Dishes
Simple DP
```py
class Solution:
    def maxSatisfaction(self, satisfaction: List[int]) -> int:
        # first is sorting, we want to make the larger one later
        # dp[i] is the max satisfaction we can have for 0 ... i
        # but we also need to know how many dishes we are in now
        # so we need dp[i][j] means the maximum satisfaction we can have for ith dishes for 0...j
        # so dp[i][j] = max(dp[i][j-1],satisfaction[j]*i+dp[i-1][j-1])
        n = len(satisfaction)
        satisfaction.sort()
        dp = [[-99999999 for i in range(n+1)] for j in range(n+1)]
        res = 0
        # if we have 0 dishes, the satisfaction will be 0
        for j in range(n+1):
            dp[0][j] = 0
        for i in range(1,n+1):
            for j in range(1,n+1):
                dp[i][j] = max(dp[i][j-1],satisfaction[j-1]*i+dp[i-1][j-1])
                res = max(dp[i][j],res)
        return res
```
Space can be reduced to O(N) since we only uses last result.