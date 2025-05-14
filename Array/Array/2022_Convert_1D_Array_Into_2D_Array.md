# 2022. Convert 1D Array Into 2D Array

| Property | Details |
|----------|--------|
| **Date Solved** | May 10, 2025 |
| **Difficulty** | Easy |
| **Topic** | Array |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/convert-1d-array-into-2d-array/description/) |

## Problem Description 
You are given a **0-indexed** 1-dimensional (1D) integer array `original`, and two integers, `m` and `n`. You are tasked with creating a 2-dimensional (2D) array with  `m` rows and `n` columns using **all** the elements from `original`.

The elements from indices `0` to `n - 1` (**inclusive**) of `original` should form the first row of the constructed 2D array, the elements from indices `n` to `2 * n - 1` (**inclusive**) should form the second row of the constructed 2D array, and so on.

Return *an* `m x n` *2D array constructed according to the above procedure, or an empty 2D array if it is impossible*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/08/26/image-20210826114243-1.png)

```
Input: original = [1,2,3,4], m = 2, n = 2
Output: [[1,2],[3,4]]
Explanation: The constructed 2D array should contain 2 rows and 2 columns.
The first group of n=2 elements in original, [1,2], becomes the first row in the constructed 2D array.
The second group of n=2 elements in original, [3,4], becomes the second row in the constructed 2D array.
```

**Example 2:**

```
Input: original = [1,2,3], m = 1, n = 3
Output: [[1,2,3]]
Explanation: The constructed 2D array should contain 1 row and 3 columns.
Put all three elements in original into the first row of the constructed 2D array.
```

**Example 3:**

```
Input: original = [1,2], m = 1, n = 1
Output: []
Explanation: There are 2 elements in original.
It is impossible to fit 2 elements in a 1x1 2D array, so return an empty 2D array.
```

**Constraints:**

- `1 <= original.length <= 5 * 104`
- `1 <= original[i] <= 105`
- `1 <= m, n <= 4 * 104`


### ✅ Summary

> I mapped the 1D array into a 2D matrix using a simple index counter.
I first checked if the dimensions matched `original == m * n` to avoid invalid input.
This early return pattern is both efficient and clean.
I chose to increment an `idx` while iterating through rows and columns to make the code easy to follow.
> 

## First Approach: Index Mapping to 2D Matrix

```java
class Solution {
    public int[][] construct2DArray(int[] original, int m, int n) {
        int o = original.length;

        if (o != (m * n)) return new int[0][0];

        int[][] answer = new int[m][n];
        int idx = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                answer[i][j] = original[idx++]; 
            }
        }

        return answer;
    }
}
```

- Time : O(m*n)
- Space : O(m*n)

### 🔎 Learning Points from the Solution

1. Check for invalid dimensions early
    - Comparing `original.length` to `m * n` before any processing avoids errors or overflows.
    - Good early-return practice.
2. Linear index mapping is effective and clear
    - Using a single index to traverse the original array makes the code clean.
    - Avoids unnecessary calculations like `i * n + j`.
