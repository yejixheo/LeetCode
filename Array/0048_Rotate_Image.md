# 48. Rotate Image

| Property | Details |
|----------|--------|
| **Date Solved** | May 20, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, Matrix, Math |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/rotate-image/description/) |

## Problem Description 
You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)

```
Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

**Constraints:**

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

### ✅ Summary

> I rotated the image 90 degrees in-place by performing four-way swaps on each square layer.
For each layer, I looped from the outermost square inward, rotating each group of four elements using index math.
This solution runs in o(n2) time, visiting each element once, and uses O(1) space as it does not require any extra memory.
Another in-place approach is to first transpose the matrix - swapping `matrix[i][j]`with `matrix[j][i]` - and then reverse each row.
> 

## First Approach: In-place 4-way rotation

```java
class Solution {
    public void rotate(int[][] matrix) {
        
        int N = matrix.length;

        for (int i = 0; i < N / 2; i++) {
            for (int j = i; j < N - 1 - i; j++) {
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[N - 1 - j][i];
                matrix[N - 1 - j][i] = matrix[N - 1 - i][N - 1 - j];
                matrix[N - 1 - i][N - 1 - j] = matrix[j][N - 1 - i];
                matrix[j][N - 1- i] = tmp;
            }
        }
    }
}
```

- Time : O(n2)
- Space : O(1)

### 🔎 Learning Points from the Solution

1. **Index math is a bit tricky**
    - `matrix[i][j] -> matrix[j][N-1-i]` type of mapping must be carefully managed.
2. **Only works for square matrices**
    - This algorithm assumes an n x n shape.

## Second Approach: Transpose + Reverse

- Transpose the matrix (flip across diagonal) and then reverse each row.

```java
public void rotate(int[][] matrix) {
    int n = matrix.length;

    // Step 1: Transpose
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int tmp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = tmp;
        }
    }

    // Step 2: Reverse each row
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n / 2; j++) {
            int tmp = matrix[i][j];
            matrix[i][j] = matrix[i][n - 1 - j];
            matrix[i][n - 1 - j] = tmp;
        }
    }
}
```
