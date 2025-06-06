# 79. Word Search

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 06, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, Backtracking, DFS |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/word-search/description/) |

## Problem Description 
Given an `m x n` grid of characters `board` and a string `word`, return `true` *if* `word` *exists in the grid*.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true

```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true

```

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false

```

**Constraints:**

- `m == board.length`
- `n = board[i].length`
- `1 <= m, n <= 6`
- `1 <= word.length <= 15`
- `board` and `word` consists of only lowercase and uppercase English letters.

### ✅ Summary

> In solving this problem, I implemented a DFS with backtracking and used a separate `visited` matrix to keep track of visited cells instead of modifying the original input `board` . While modifying the input directly (in-place) is a known technique to optimize space usage, I intentionally avoided this method because I believe changing input data can introduce hidden bugs, especially in collaborative or production environments.
> 

## First Approach:

```java
class Solution {

    char[][] board;
    String word;
    int m, n;

    int[] dr = {-1, 1, 0, 0};
    int[] dc = {0, 0, -1, 1};

    public boolean exist(char[][] board, String word) {
        
        this.board = board;
        this.word = word;
        m = board.length;
        n = board[0].length;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == word.charAt(0)) {
                    boolean[][] visited = new boolean[m][n];
                    if (dfs(i, j, 0, visited)) return true;
                }
            }
        }

        return false;

    }

    private boolean dfs(int r, int c, int idx, boolean[][] visited) {
        if (idx == word.length() - 1) return true;

        visited[r][c] = true;
        idx++;

        for (int d = 0; d < 4; d++) {
            int nr = r + dr[d];
            int nc = c + dc[d];
            if (nr >= 0 && nc >= 0 && nr < m && nc < n && !visited[nr][nc] && board[nr][nc] == word.charAt(idx)){
                if (dfs(nr, nc, idx, visited)) return true;
            }
        }

        visited[r][c] = false;
        return false;
    }
}
```
