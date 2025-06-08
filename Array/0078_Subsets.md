# 78.Subsets

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 07, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, Backtracking |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/subsets/description/) |

## Problem Description 
Given an integer array `nums` of **unique** elements, return *all possible* *subsets* *(the power set)*.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

```

**Example 2:**

```
Input: nums = [0]
Output: [[],[0]]

```

**Constraints:**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- All the numbers of `nums` are **unique**.

### ✅ Summary

> This solution uses DFS backtracking with a `startIndex` to avoid duplicate subsets. A copy of the temporary list is added to the result to avoid reference issues.
> 

## First Approach: DFS

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        dfs(res, 0, new ArrayList<>(), nums);

        return res;
    }

    public void dfs(List<List<Integer>> res, int idx, List<Integer> tmp, int[] nums) {
        res.add(new ArrayList<>(tmp));

        for (int i = idx; i < nums.length; i++) {
            tmp.add(nums[i]);
            dfs(res, i + 1, tmp, nums);
            tmp.remove(tmp.size() - 1);
        }
    }
}
```

- Time : O(2ⁿ)
- Space : O(2ⁿ)
