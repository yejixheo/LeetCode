# 90. Subsets2

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 07, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, Backtracking |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/subsets-ii/description/) |

## Problem Description 
Given an integer array `nums` that may contain duplicates, return *all possible* *subsets (the power set)*.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

```
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**Example 2:**

```
Input: nums = [0]
Output: [[],[0]]
```

**Constraints:**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`

### ✅ Summary

> In this problem, what I did first was sort the input array. The reason I did was to make it easier to detect and skip duplicate values during backtracking. While generating subsets, I made sure to skip the second and later instances of a duplicate at the same recursive depth by using the condition. This allowed me to generate all uniques subsets without needing to use extra data structures like HashSet.
> 

## First Approach:

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);
        generateSubset(nums, res, new ArrayList<>(), 0);

        return res;
    }

    public void generateSubset(int[] nums, List<List<Integer>> res, List<Integer> tmp, int idx) {
        res.add(new ArrayList<>(tmp));

        for (int i = idx; i < nums.length; i++) {
            if (i > idx && nums[i] == nums[i - 1]) continue;

            tmp.add(nums[i]);
            generateSubset(nums, res, tmp, i + 1);
            tmp.remove(tmp.size() - 1);
        }
    }
}
```
