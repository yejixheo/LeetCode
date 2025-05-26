# 200. Number of Islands

| Property | Details |
|----------|--------|
| **Date Solved** | May 25, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, DFS, BFS |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/number-of-islands/post-solution/?submissionId=1644499553) |

## Problem Description 
Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1

```

**Example 2:**

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

## First Approach: BFS

```java
class Solution {

    public int numIslands(char[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        boolean[][] visited = new boolean[m][n];
        int cnt = 0;
         
        for (int r = 0; r < m; r++) {
            for (int c = 0; c < n; c++) {
                if(grid[r][c] == '1' && !visited[r][c]) {
                    cnt++;
                    visited[r][c] = true;
                    Queue<int[]> q = new LinkedList<>();
                    q.offer(new int[]{r, c});
                    bfs(grid, visited, q);
                }
            }
        }
        
        return cnt;
    }

    public void bfs(char[][] grid, boolean[][] visited, Queue<int[]> q) {
        
        int[] dr = {-1, 1, 0, 0};
        int[] dc = {0, 0, -1, 1};

        int m = grid.length;
        int n = grid[0].length;

        while (!q.isEmpty()) {
            
            int[] curr = q.poll();
            int r = curr[0];
            int c = curr[1];
            
            for (int d = 0; d < 4; d++) {
                int nr = r + dr[d];
                int nc = c + dc[d];

                if (nr < 0 || nc < 0 || nr >= m || nc >= n || visited[nr][nc] || grid[nr][nc] == '0') continue;
                visited[nr][nc] = true;
                q.offer(new int[]{nr, nc});
            }

        }
    }
}
```

- Time : O(m x n)
- Space : O(m x n)

## Second Approach: DFS

```java
class Solution {
    
    int m, n;
    boolean[][] visited;

    int[] dr = {-1, 1, 0, 0};
    int[] dc = {0, 0, -1, 1};

    public int numIslands(char[][] grid) {
        m = grid.length;
        n = grid[0].length;
        visited = new boolean[m][n];
        int cnt = 0;
         
        for (int r = 0; r < m; r++) {
            for (int c = 0; c < n; c++) {
                if(grid[r][c] == '1' && !visited[r][c]) {
                    cnt++;
                    dfs(grid, r, c);
                }
            }
        }
        
        return cnt;
    }

    public void dfs(char[][] grid, int r, int c) {
        visited[r][c] = true;

        for (int d = 0; d < 4; d++) {
            int nr = r + dr[d];
            int nc = c + dc[d];

            if(nr < 0 || nc < 0 || nr >= m || nc >= n || visited[nr][nc] || grid[nr][nc] == '0') continue;
            else dfs(grid, nr, nc);
        }
    }
}
```
