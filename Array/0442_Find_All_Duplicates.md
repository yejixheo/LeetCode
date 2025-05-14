# 442. Find All Duplicates in an Array

| Property | Details |
|----------|--------|
| **Date Solved** | May 14, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/find-all-duplicates-in-an-array/description/) |

## Problem Description 
Given an integer array `nums` of length `n` where all the integers of `nums` are in the range `[1, n]` and each integer appears **at most** **twice**, return *an array of all the integers that appears **twice***.

You must write an algorithm that runs in `O(n)` time and uses only *constant* auxiliary space, excluding the space needed to store the output

**Example 1:**

```
Input: nums = [4,3,2,7,8,2,3,1]
Output: [2,3]
```

**Example 2:**

```
Input: nums = [1,1,2]
Output: [1]
```

**Example 3:**

```
Input: nums = [1]
Output: []
```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 105`
- `1 <= nums[i] <= n`
- Each element in `nums` appears **once** or **twice**.

### ✅ Summary

> I solved this problem using in-place negation. 
For each value in the array, I used `idx = Math.abs(num[i])` .
If `nums[idx - 1] < 0`, then this value has already appeared - I add answer `idx` to the result.
Otherwise, I flip the sign of `nums[idx -1]` to mark it as visited.
> 

## First Approach: In-place Marking Using Negatives

```java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        
        List<Integer> answer = new ArrayList<>();

        if (nums.length < 2) return answer;

        for (int num : nums) {
            int idx = num;
            if (idx < 0 ) idx *= -1;
            idx -= 1;
            if (nums[idx] < 0) answer.add(idx + 1);
            else nums[idx] *= -1;
        }

        return answer;

    }
}
```

- Time : O(n)
- Space : O(1)

### 🔎 Learning Points from the Solution

1. **Can be slightly optimized for clarity**
    - Instead of manually flipping signs, can use `Math.abs(num)` for readability.
    - Better to inline indexing logic without toggling `-1, +1` separately.
     (`idx -= 1` → `idx - 1` / `add(idx + 1)` → `add(idx)`)

```java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
    
        List<Integer> answer = new ArrayList<>();
        int n = nums.length;
        
        for (int i = 0; i < n; i++) {
            int idx = Math.abs(nums[i]);
            if (nums[idx - 1] < 0) {
                answer.add(idx);
            }
            nums[idx - 1] *= -1;
        }
        return answer;
    }
}
```
