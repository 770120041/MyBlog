---
layout: post
title:  Python Graph, Union find
categories: Interview
---
## 133. Clone Graph
Using dict

```py
class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        if not node: return node
        d = {}
        def visitGraph(node,d):
            if node in d: return d[node]
            d[node] = Node(node.val)
            for neighbor in node.neighbors:
                d[node].neighbors.append(visitGraph(neighbor,d))
            return d[node]
        
        return visitGraph(node,d)
```

## 310. Minimum Height Trees
1. need a matrix and a indegree array
2. topo sort until there are 1 or 2 numbers in the queue, those are the roots
https://leetcode.com/problems/minimum-height-trees/discuss/76052/Two-O(n)-solutions

拓朴排序TLE了
```py
class Solution:
    def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
        if n == 1: return [0]
        mat = [[False for i in range(n)] for j in range(n)]
        indegree = [0]*n
        for edge in edges:
            indegree[edge[0]]+=1
            indegree[edge[1]]+=1
            mat[edge[0]][edge[1]] = mat[edge[1]][edge[0]] = True
        q = []
        for i in range(n):
            if indegree[i] == 1:
                q.append(i)
        cnt = n
        # if there are more than 2 nodes left. We try to pop numbers out
        while cnt > 2 :
            sz = len(q)
            cnt -= sz
            for i in range(sz):
                cur = q.pop(0)
                for j in range(n):
                    if mat[j][cur]:
                        indegree[j]-=1
                        if indegree[j] == 1:
                            q.append(j)
        return q
```

## 399. Evaluate Division
```py
class Solution:
    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        d = {}
        for i in range(len(values)):
            if equations[i][0] not in d: d[equations[i][0]] = []
            if equations[i][1] not in d: d[equations[i][1]] = []
            d[equations[i][0]].append((equations[i][1],values[i]))
            d[equations[i][1]].append((equations[i][0],1/values[i]))
        def dfs(cur,target,val,visited):
            if cur not in d: return -1.0
            if cur == target: return val
            visited.add(cur)
            for neighbor in d[cur]:
                if neighbor[0] not in visited:
                    ret = dfs(neighbor[0],target,val*neighbor[1],visited) 
                    if ret != -1.0: return ret
            visited.remove(cur)
            return -1.0
        res = []
        for query in queries:
            res += [dfs(query[0],query[1],1.0,set())]
        return res
```

## (lint) 178. Graph Valid Tree
判断是不是树
1.是不是所有点都有相同的根节点
2.如果有两个还没遇到的边的根节点是同一个点，说明这个边构成了环（因为它们已经连接到同一个根了），所以不是一个完整的树。
```py
class Solution:
    # union find, if it is from same root. then false
    
    def validTree(self, n, edges):
        def findRoot(pre,i):
            if pre[i] != i:
                pre[i] = findRoot(pre,pre[i])
            return pre[i]
        pre = [0]*n
        for i in range(n): pre[i] = i
        for edge in edges:
            x,y = edge[0],edge[1]
            rootx,rooty = findRoot(pre,x), findRoot(pre,y)
            # print(x,y,rootx,rooty)
            if rooty == rootx: return False
            pre[rootx] = rooty
        for i in range(0,n):
            if findRoot(pre,i) != findRoot(pre,i-1): return False
        return True
            
```

## 434. Number of Islands II
因为要return number of islands after each opeartion， dfs复杂度太高，所以可以判断每次添加一个点之后会不会和之前的island联通，如果是的话就不增加island，否则增加island。判断邻居是不是island，如果是的话连起来就行。但有一个问题是如果连起来以后使得原来两个不相邻的island联通了， 那么island的数量就少了一个。所以需要使用union find.并且简单的处理对于
```
1 1 0
1 0 0
1 1 0
```
的情况也不是很好处理。

每次添加island以后，做union，然后对所有添加过的island来findroot

## 323	Number of Connected Components in an Undirected Graph
并查集或者dfs都可以. DFS的话用visited 数组标记每个被visit的点。

并查集的话用pre数组定义好根


## 434. Number of Islands II
因为要return number of islands after each opeartion， dfs复杂度太高，所以可以判断每次添加一个点之后会不会和之前的island联通，如果是的话就不增加island，否则增加island。判断邻居是不是island，如果是的话连起来就行。但有一个问题是如果连起来以后使得原来两个不相邻的island联通了， 那么island的数量就少了一个。所以需要使用union find.并且简单的处理对于
```
1 1 0
1 0 0
1 1 0
```
的情况也不是很好处理。

每次添加island以后，做union，然后对所有添加过的island来findroot

Lintcode好像test case 有错误
```py
"""
Definition for a point.
class Point:
    def __init__(self, a=0, b=0):
        self.x = a
        self.y = b
"""

class Solution:
    def numIslands2(self, n, m, operators):
        
        pre = {}
        cur = 0
        res = []

        def findRoot(pre,a, n):
            if pre[a] != a[0]*(n+100)+a[1]:
                pre[a] = findRoot(pre,pre[a],n)
            return pre[a]
            
        dir = ((-1,0),(1,0),(0,-1),(0,1))
        for operator in operators:
            x,y = operator.x,operator.y
            if (x,y) not in pre:
                pre[(x,y)] = x*(n+100)+y
                cur += 1
                for i in range(4):
                    nextX,nextY = x+dir[i][0], y+dir[i][1]
                    if nextX<0 or nextX >= m or nextY<0 or nextY >= n or (nextX,nextY) not in pre:
                        continue
                    rootA,rootB = findRoot(pre,(x,y),n),findRoot(pre,(nextX,nextY),n)
                    print((x,y),(nextX,nextY),rootA,rootB)
                    if rootA != rootB: cur-=1
                    pre[rootA] = rootB
            res.append(cur)
        return res

```

## 1377. Frog Position After T Seconds

```py
class Solution:
    def frogPosition(self, n: int, edges: List[List[int]], t: int, target: int) -> float:
        # DFS from first node
        # same way, use adj list to represent a tree
        def dfs(pos,dis,children,val,possi=1,visited=set()):
            num_child = len(children[pos])
            if dis == 0 or num_child == 0: 
                val[pos] = possi
                return
            for child in children[pos]:
                if child not in visited:
                    children[child].remove(pos)
                    visited.add(child)
                    dfs(child,dis-1,children,val,possi*num_child,visited)
                    visited.remove(child)
                    children[child].add(pos)

            
        children = [set() for i in range(n+1)]
        val= [0.0 for i in range(n+1)]
        for edge in edges:
            children[edge[0]].add(edge[1])
            children[edge[1]].add(edge[0])
        dfs(1,t,children,val)
        print(val)
        if val[target] == 0: return 0
        return 1/val[target]
```