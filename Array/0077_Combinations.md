# 77. Combinations

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 28, 2025 |
| **Difficulty** | Medium |
| **Topic** | Backtracking |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/combinations/description/) |

## Problem Description 
Given two integers `n` and `k`, return *all possible combinations of* `k` *numbers chosen from the range* `[1, n]`.

You may return the answer in **any order**.

**Example 1:**

```
Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Explanation: There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.
```

**Example 2:**

```
Input: n = 1, k = 1
Output: [[1]]
Explanation: There is 1 choose 1 = 1 total combination.
```

**Constraints:**

- `1 <= n <= 20`
- `1 <= k <= n`

### ✅ Summary

> I used backtracking to explore combinations of k numbers from 1 to n. At each step, I add the next possible number and recurse, stopping when the combination reaches size k. To avoid duplicates, I make sure each next number starts from `i + 1` , ensuring increasing order. This approach guarantees all unique combinations without repetition and is efficient for small n.
> 

## First Approach: Backtracking

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(res, new ArrayList<>(), 1, n, k);
        return res;
    }

    private void backtrack(List<List<Integer>> res, List<Integer> tmp, int start, int max, int remain) {
        if (remain == 0) {
            res.add(new ArrayList<>(tmp));
            return;
        }

        for (int i = start; i <= max; i++) {
            tmp.add(i);
            backtrack(res, tmp, i + 1, max, remain - 1);
            tmp.remove(tmp.size() - 1);
        }
    }
}
```
