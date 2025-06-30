# 162. Find Peak Element

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 30, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, Binary Search |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/find-peak-element/description/) |

## Problem Description
A peak element is an element that is strictly greater than its neighbors.

Given a **0-indexed** integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that `nums[-1] = nums[n] = -∞`. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in `O(log n)` time.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

**Example 2:**

```
Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
```

**Constraints:**

- `1 <= nums.length <= 1000`
- `-231 <= nums[i] <= 231 - 1`
- `nums[i] != nums[i + 1]` for all valid `i`.

### ✅ Summary

> I’m using binary search to find a peak element. At each step, I compare `nums[mid]` and `nums[mid + 1]` . If `nums[mid] > nums[mid + 1]` , that means a peak lies on the left side including mid, so I move the right pointer to mid. Otherwise, I move the left pointer to `mid + 1` .
> 

## First Approach:

```java
class Solution {
    public int findPeakElement(int[] nums) {

        int left = 0;
        int right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] > nums[mid + 1]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        
        return left;
    }
}

```
