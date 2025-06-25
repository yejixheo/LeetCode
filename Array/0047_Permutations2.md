# 47. Permutations 2

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 25, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, Backtracking |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/permutations-ii/description/) |

## Problem Description 
Given a collection of numbers, `nums`, that might contain duplicates, return *all possible unique permutations **in any order**.*

**Example 1:**

```
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**Example 2:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Constraints:**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`

### ✅ Summary

> First of all, I sorted the input array to handle duplicates during the traversal. Then, I used backtracking to generate all permutations. To avoid revisiting the same element, I used `boolean[]` array to keep track of visited indices. Since the input array may contain duplicate numbers, I added an extra condition to skip duplicates and prevent generating duplicate permutations.
> 

## First Approach: Backtracking

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        backtrack(res, new ArrayList<>(), 0, nums, visited);
        return res;
    }

    private void backtrack(List<List<Integer>> res, List<Integer> tmp, int idx, int[] nums, boolean[] visited) {
        
        if (idx == nums.length) {
            res.add(new ArrayList<>(tmp));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (visited[i] || (i > 0 && !visited[i - 1] && nums[i] == nums[i - 1])) continue;
            tmp.add(nums[i]);
            visited[i] = true;
            backtrack(res, tmp, idx + 1, nums, visited);
            visited[i] = false;
            tmp.remove(tmp.size() - 1);
        }

    }
}
```
