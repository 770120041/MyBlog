---
layout: post
title:  Python BackTracking BFS&DFS
categories: Interview
---
## 200 islands
```py
class Solution(object):
    def numIslands(self, grid):
        def sink(i, j):
            if 0 <= i < len(grid) and 0 <= j < len(grid[i]) and grid[i][j] == '1':
                grid[i][j] = '0'
                map(sink, (i+1, i-1, i, i), (j, j, j+1, j-1))
                return 1
            return 0
        return sum(sink(i, j) for i in range(len(grid)) for j in range(len(grid[i])))

```

```py
class Solution(object):
    def numIslands(self, grid):
        def helper(g,i,j):
            if i < 0 or j < 0 or i >= len(g) or j >= len(g[0]) or g[i][j] == "0": return
            g[i][j] = "0"
            helper(g,i-1,j)
            helper(g,i+1,j)
            helper(g,i,j-1)
            helper(g,i,j+1)
        cnt = 0
        for x in range(len(grid)):
            for y in range(len(grid[0])):
                if grid[x][y] == "1":
                    cnt+=1
                    helper(grid,x,y)
        return cnt
```

## 663. Walls and Gates
