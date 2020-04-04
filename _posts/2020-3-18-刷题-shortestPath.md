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
