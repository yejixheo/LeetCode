# 73. Set Matrix Zeroes

| Property | Details |
|----------|--------|
| **Date Solved** | May 15, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, HashTable, Matrix |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/set-matrix-zeroes/description/) |

## Problem Description 
Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

```
Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
Output: [[1,0,1],[0,0,0],[1,0,1]]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

```
Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```

**Constraints:**

- `m == matrix.length`
- `n == matrix[0].length`
- `1 <= m, n <= 200`
- `-231 <= matrix[i][j] <= 231 - 1`

**Follow up:**

- A straightforward solution using `O(mn)` space is probably a bad idea.
- A simple improvement uses `O(m + n)` space, but still not the best solution.
- Could you devise a constant space solution?

### ✅ Summary

> I first implemented a solution that saves the positions of all zeroes and then propagates them across their row and column using 4-directional while loops.
But this leads to `O(m x n x (m + n))` time complexity, which is inefficient for large matrics.
A better approach is to mark the rows and columns with boolean arrays, resulting in `O(m + n)` space and `O(m x n)` time.
The most optimal solution uses the first row and column as internal markers with two booleans to track whether they should be zeroed.
This final method uses `O(1)` space and is fully in-place.
> 

## First Approach:

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        
        int[] dr = {-1, 1, 0, 0};
        int[] dc = {0, 0, -1, 1};

        int m = matrix.length;
        int n = matrix[0].length;

        Queue<Integer[]> zeroCount = new ArrayDeque<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0){
                    zeroCount.add(new Integer[]{i, j});
                }
            }
        }

        while (!zeroCount.isEmpty()) {
            Integer[] now = zeroCount.poll();
            int r = now[0];
            int c = now[1];
            for (int d = 0; d < 4; d++) {
                int nr = r + dr[d];
                int nc = c + dc[d];
                while (nr >= 0 && nc >= 0 && nr < m && nc < n) {
                    matrix[nr][nc] = 0;
                    nr += dr[d];
                    nc += dc[d];
                }
            }
        }
    }
}
```

- Time : O(m X n (m + n)) → inefficient for large grids
- Space : O(k) → numbers of zeroes in input

### 🔎 Learning Points from the Solution

1. **Expands in all four directions manually**
    - Using while-loops per direction is logically valid but verbose and inefficient.
2. **Use of `Integer[]` is unnecessarily heavy**
    - `int[]` or coordinate class is more efficient.
3. **Rewriting same rows/columns multiple times.**
    - Repeated 0 propagation for each zero causes redundant work.

## Second Approach: Boolean Row and Column Marking

- Step1 : Marks rows and cols that need to be zeroed.
- Step2 : Set all marked rows and columns to 0.
- O(m+n) extra space used, still not the best solution.

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        
        int m = matrix.length;
        int n = matrix[0].length;

        boolean[] row = new boolean[m];
        boolean[] col = new boolean[n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    row[i] = true;
                    col[j] = true;
                }
            }
        }

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (row[i] || col[j]) matrix[i][j] = 0;
            }
        }

    }
}
```

## Third Approach: In-place using first row/col as markers

- Use the first row and column to mark which rows/columns to zero.
- Track whether the first row/column should be zeroed separately.
```java
class Solution {
    public void setZeroes(int[][] matrix) {
        
        int m = matrix.length;
        int n = matrix[0].length;

        boolean firstRow = false;
        boolean firstCol = false;

        for (int i = 0; i < m; i++) if(matrix[i][0] == 0) firstCol = true;
        for (int j = 0; j < n; j++) if(matrix[0][j] == 0) firstRow = true;

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if(matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) matrix[i][j] = 0;
            }
        }

        if (firstCol) for(int i = 0; i < m; i++) matrix[i][0] = 0;
        if (firstRow) for(int j = 0; j < n; j++) matrix[0][j] = 0;

    }
}
```
