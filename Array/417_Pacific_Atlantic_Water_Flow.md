# 417. Pacific Atlantic Water Flow

| Property | Details |
|----------|--------|
| **Date Solved** | May 23, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, DFS, BFS |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/pacific-atlantic-water-flow/description/) |

## Problem Description 
There is an `m x n` rectangular island that borders both the **Pacific Ocean** and **Atlantic Ocean**. The **Pacific Ocean** touches the island's left and top edges, and the **Atlantic Ocean** touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the **height above sea level** of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is **less than or equal to** the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return *a **2D list** of grid coordinates* `result` *where* `result[i] = [ri, ci]` *denotes that rain water can flow from cell* `(ri, ci)` *to **both** the Pacific and Atlantic oceans*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/08/waterflow-grid.jpg)

```
Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
Explanation: The following cells can flow to the Pacific and Atlantic oceans, as shown below:
[0,4]: [0,4] -> Pacific Ocean
       [0,4] -> Atlantic Ocean
[1,3]: [1,3] -> [0,3] -> Pacific Ocean
       [1,3] -> [1,4] -> Atlantic Ocean
[1,4]: [1,4] -> [1,3] -> [0,3] -> Pacific Ocean
       [1,4] -> Atlantic Ocean
[2,2]: [2,2] -> [1,2] -> [0,2] -> Pacific Ocean
       [2,2] -> [2,3] -> [2,4] -> Atlantic Ocean
[3,0]: [3,0] -> Pacific Ocean
       [3,0] -> [4,0] -> Atlantic Ocean
[3,1]: [3,1] -> [3,0] -> Pacific Ocean
       [3,1] -> [4,1] -> Atlantic Ocean
[4,0]: [4,0] -> Pacific Ocean
       [4,0] -> Atlantic Ocean
Note that there are other possible paths for these cells to flow to the Pacific and Atlantic oceans.
```

**Example 2:**

```
Input: heights = [[1]]
Output: [[0,0]]
Explanation: The water can flow from the only cell to the Pacific and Atlantic oceans.
```

**Constraints:**

- `m == heights.length`
- `n == heights[r].length`
- `1 <= m, n <= 200`
- `0 <= heights[r][c] <= 105`

### ✅ Summary

> I initially considered running DFS from every cell to determine whether it could reach both oceans, but I anticipated significant performance issues due to redundant traversal. To avoid unnecessary computation, I reversed the logic : rather than simulating water flow from each cell to the oceans, I simulated water flowing inland from the oceans. This approach allowed me to explore only reachable paths once per ocean and significantly reduced the time complexity. I implemented both DFS and BFS versions: DFS is concise and intuitive, while BFS avoids recursion and handles deep grids more safely.
> 

## First Approach: DFS from Ocean Borders

- Start DFS from Pacific and Atlantic edges, not from each cell.
- Mark all cells that each ocean can reach, then find the intersection.

```java
class Solution {

    int m, n;

    int[] dr = {-1, 1, 0, 0};
    int[] dc = {0, 0, -1, 1};

    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        
        m = heights.length;
        n = heights[0].length;

        boolean[][] pacific = new boolean[m][n];
        boolean[][] atlantic = new boolean[m][n];

        for (int r = 0; r < m; r++) {
            dfs(heights, pacific, r, 0);
            dfs(heights, atlantic, r, n - 1);
        }

        for (int c = 0; c < n; c++) {
            dfs(heights, pacific, 0, c);
            dfs(heights, atlantic, m - 1, c);
        }

        List<List<Integer>> res = new ArrayList<>();
        for (int r = 0; r < m; r++) {
            for (int c = 0; c < n; c++) {
                if(pacific[r][c] && atlantic[r][c]) res.add(List.of(r, c));
            }
        }

        return res;
    }

    public void dfs(int[][] heights, boolean[][] visited, int r, int c) {
        visited[r][c] = true;

        for (int d = 0; d < 4; d++) {
            int nr = r + dr[d];
            int nc = c + dc[d];
            
            if (nr < 0 || nc < 0 || nr >= m || nc >= n || visited[nr][nc] || heights[r][c] > heights[nr][nc]) continue;
            dfs(heights, visited, nr, nc);
        }
    }
}
```

- Time : O(m x n)
- Space : O(m x n)

### 🔎 Learning Points from the Solution

1. **DFS from every cell leads to repetitive work → can be inverted**
    - Reverse thinking: from ocean → cell, not cell → ocean

## Second Approach: BFS from Ocean Borders

- Use a queue to simulate water spreading outward from the oceans.
- For each ocean, enqueue all its border cells and mark where water can flow.

```java
class Solution {

    int m, n;

    int[] dr = {-1, 1, 0, 0};
    int[] dc = {0, 0, -1, 1};

    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        
        m = heights.length;
        n = heights[0].length;

        boolean[][] pacific = new boolean[m][n];
        boolean[][] atlantic = new boolean[m][n];

        Queue<int[]> pq = new LinkedList<>();
        Queue<int[]> aq = new LinkedList<>();

        for (int r = 0; r < m; r++) {
            pq.offer(new int[]{r, 0});
            aq.offer(new int[]{r, n - 1});
            pacific[r][0] = true;
            atlantic[r][n - 1] = true;
        }

        for (int c = 0; c < n; c++) {
            pq.offer(new int[]{0, c});
            aq.offer(new int[]{m - 1, c});
            pacific[0][c] = true;
            atlantic[m - 1][c] = true;
        }

        bfs(heights, pacific, pq);
        bfs(heights, atlantic, aq);

        List<List<Integer>> res = new ArrayList<>();

        for (int r = 0; r < m; r++) {
            for (int c = 0; c < n; c++) {
                if (pacific[r][c] && atlantic[r][c]) res.add(List.of(r, c));
            }
        }

        return res;

    }

    public void bfs(int[][] heights, boolean[][] visited, Queue<int[]> q) {

        while (!q.isEmpty()) {

            int[] curr = q.poll();
            int r = curr[0], c = curr[1];

            for (int d = 0; d < 4; d++) {
                int nr = r + dr[d];
                int nc = c + dc[d];

                if (nr < 0 || nc < 0 || nr >= m || nc >= n || visited[nr][nc] || heights[nr][nc] < heights[r][c]) continue;
                visited[nr][nc] = true;
                q.offer(new int[]{nr, nc});
            }

        }

    }

}
```
