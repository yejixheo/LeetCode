# 283. Move Zeroes

| Property | Details |
|----------|--------|
| **Date Solved** | May 11, 2025 |
| **Difficulty** | Easy |
| **Topic** | Array |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/move-zeroes/description/) |

## Problem Description
Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

**Example 1:**

```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Example 2:**

```
Input: nums = [0]
Output: [0]
```

**Constraints:**

- `1 <= nums.length <= 104`
- `231 <= nums[i] <= 231 - 1`

**Follow up:**

Could you minimize the total number of operations done?

### ✅ Summary

> I used a single pointer strategy that efficiently moves all non-zero values to the front.
After that, I fill the remaining elements with zeros.
This ensures stable order without unnecessary swaps.
One thing I would improve is to include a defensive check like if `(nums == null || nums.length == 0)` to avoid unnecessary work and improve robustness.
> 

## First Approach: Index Pointer

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int idx = 0;

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) nums[idx++] = nums[i];
        }

        for(int i = idx; i < nums.length; i++) {
            nums[i] = 0;
        }
    }
}
```

- Time : O(n)
- Space :  O(1)

### 🔎 Learning Points from the Solution

1. **Missing defensive check for null or empty input**
    - Even if problem constraints say it won’t happen, it shows robustness and best practices.
