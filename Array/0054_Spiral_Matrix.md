# 54. Spiral Matrix

| Property | Details |
|----------|--------|
| **Date Solved** | May 16, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, Matrix, Simulation |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/spiral-matrix/description/) |

## Problem Description 
Given an `m x n` `matrix`, return *all elements of the* `matrix` *in spiral order*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]

```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

```
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]

```

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`

### ✅ Summary

> I chose to simulate movement using direction arrays(`dr`, `dc` ) to keep the logic simple and scalable. To prevent revisiting the same cell, I used `visited` matrix, where each visited cell is marked as true.
When hitting a boundary or visited cell, I rotated the direction which ensures the path continues in spiral fashion.
A more optimal solution would be to use boundary pointers and traverse layer by layer without extra memory - keeping space at `O(1)` .
> 

## First Approach: Directional Traversal with Visited Tracking

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        
        List<Integer> answer = new ArrayList<>();

        int[] dr = {0, 1, 0, -1};
        int[] dc = {1, 0, -1, 0};
        int d = 0;

        int m = matrix.length;
        int n = matrix[0].length;

        boolean[][] visited = new boolean[m][n];

        int r = 0; 
        int c = 0;

        int max = m * n;
        int cnt = 0;

        while (cnt < max) {
            visited[r][c] = true;
            answer.add(matrix[r][c]);
            cnt++;

            int nr = r + dr[d];
            int nc = c + dc[d];

            if (nr >= 0 && nr < m && nc >= 0 && nc < n && !visited[nr][nc]) {
                r = nr;
                c = nc;
            } else {
                d = (d + 1) % 4;
                r = r + dr[d];
                c = c + dc[d];
            }
        }

        return answer;
    }
}
```

- Time : O(m x n)
- Space : O(m x n)

### 🔎 Learning Points from the Solution

1. **Space can be optimized further**
    - Instead of visited array, can track matrix boundaries.

## Second Approach: Boundary Shrinking

- Shrink the matrix boundaries layer by layer while collecting elements in spiral order.

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        
        List<Integer> answer = new ArrayList<>();

        int top = 0;
        int bottom = matrix.length - 1;
        int left = 0;
        int right = matrix[0].length - 1;

        while (top <= bottom && left <= right) {
            for (int c = left; c <= right; c++) answer.add(matrix[top][c]);
            top++;
            for (int r = top; r <= bottom; r++) answer.add(matrix[r][right]);
            right--;
            if (top <= bottom) {
                for (int c = right; c >= left; c--) answer.add(matrix[bottom][c]);
                bottom--;
            }
            if (left <= right) {
                for (int r = bottom; r >= top; r--) answer.add(matrix[r][left]);
                left++;
            }
        } 

        return answer;
    }
}
```
