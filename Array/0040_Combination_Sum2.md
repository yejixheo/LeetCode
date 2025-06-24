# 40. Combination Sum 2

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 23, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, Backtracking |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/combination-sum-ii/description/) |

## Problem Description 
Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5
Output:
[
[1,2,2],
[5]
]
```

**Constraints:**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

### ✅ Summary

> I implemented a backtracking solution by first sorting the input array to manage duplicates. At each recursive level, I skip values that are the same as the previous to avoid repeated combinations, using the condition `i > idx && candidates[i] == candidates[i - 1]` .
> 

## First Approach

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(candidates);
        backtrack(res, new ArrayList<>(), candidates, target, 0);
        return res;
    }

    private void backtrack(List<List<Integer>> res, List<Integer> tmp, int[] candidates, int remain, int idx) {

        if (remain == 0) {
            res.add(new ArrayList<>(tmp));
            return;
        }

        if (idx >= candidates.length || remain < 0 || candidates[idx] > remain) return;

        for (int i = idx; i < candidates.length; i++) {
            if (i > idx && candidates[i] == candidates[i - 1]) continue;

            tmp.add(candidates[i]);
            backtrack(res, tmp, candidates, remain - candidates[i], i + 1);
            tmp.remove(tmp.size() - 1);
        }
    }
}
```
