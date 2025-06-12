# 39. Combination Sum

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 12, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, Backtracking |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/combination-sum/description/) |

## Problem Description 
Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

**Example 3:**

```
Input: candidates = [2], target = 1
Output: []
```

**Constraints:**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 40`

### ✅ Summary

> I solved this problem using backtracking to explore all combinations of candidates. At first, I tracked the current sum explicitly in each recursive call, which made the logic more verbose. So I replaced it with a `remain` variable to represent the remaining value  to reach the target. This change reduced complexity and made the recursion cleaner.
> 

## First Approach: Backtracking

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        generateCom(candidates, target, res, new ArrayList<>(), 0, 0);
        return res;
    }

    private void generateCom(int[] candidates, int target, List<List<Integer>> res, List<Integer> tmp, int idx, int sum) {
        if (sum == target) {
            res.add(new ArrayList(tmp));
            return;
        } else if(sum > target) return;

        for (int i = idx; i < candidates.length; i++) {
            sum += candidates[i];
            tmp.add(candidates[i]);
            generateCom(candidates, target, res, tmp, i, sum);
            tmp.remove(tmp.size() - 1);
            sum -= candidates[i];
        }
    }
}
```

### 🔎 Learning Points from the Solution

1. **Replace `sum` tracking with `remain` for clarity**
    - Using `remain` avoids manually increasing/decreasing `sum` and keeps logic simpler.
    - can reduce the number of parameters and improves readability.
2. **Early stopping with sorted candidates**
    - If `candidates`is sorted, you can break early in the loop when `candidates[i] > remain`

## Improved Version

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(candidates);
        backtrack(res, new ArrayList<>(), candidates, target, 0);
        return res;
    }

    private void backtrack(List<List<Integer>> res, List<Integer> tmp, int[] candidates, int remain, int start) {
        if (remain < 0) return;
        if (remain == 0) {
            res.add(new ArrayList<>(tmp));
            return;
        }
        for (int i = start; i < candidates.length; i++) {
            if (candidates[i] > remain) break;
            tmp.add(candidates[i]);
            backtrack(res, tmp, candidates, remain - candidates[i], i);
            tmp.remove(tmp.size() - 1);
        }
    }
}
```
