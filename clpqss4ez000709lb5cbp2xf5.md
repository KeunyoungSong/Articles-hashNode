---
title: "[Leetcode]200. Number of Islands"
datePublished: Mon Dec 04 2023 10:57:46 GMT+0000 (Coordinated Universal Time)
cuid: clpqss4ez000709lb5cbp2xf5
slug: leetcode200-number-of-islands

---

### Key Idea
- Extract 'row' and 'col' from the entire grid.
- Traverse all nodes, perform DFS when the value is 1.
- Within DFS, check boundary and perform DFS for nodes in all four directions from that position    

```kotlin
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        row, col = len(grid[0]), len(grid)
        cnt = 0

        def dfs(x,y):
            if x < 0 or x >= row: return
            if y < 0 or y >= col: return

            if grid[y][x] == '1':
                grid[y][x] = 0

                dfs(x-1, y)
                dfs(x, y-1)
                dfs(x+1, y)
                dfs(x, y+1)
        
        for y in range(col):
            for x in range(row):
                if grid[y][x] == '1':
                    dfs(x, y)
                    cnt += 1
                    
        return cnt
```
%[https://github.com/LB-Brandon/algorithm/tree/main/0200-number-of-islands]