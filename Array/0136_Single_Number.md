# 136. Single Number

| Property | Details |
|----------|--------|
| **Date Solved** | May 9, 2025 |
| **Difficulty** | Easy |
| **Topic** | Array, BitManipulation |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/single-number/description/) |

## Problem Description 
Given a **non-empty** array of integers `nums`, every element appears *twice* except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

**Example 1:**

**Input:** nums = [2,2,1]

**Output:** 1

**Example 2:**

**Input:** nums = [4,1,2,1,2]

**Output:** 4

**Example 3:**

**Input:** nums = [1]

**Output:** 1

**Constraints:**

- `1 <= nums.length <= 3 * 104`
- `3 * 104 <= nums[i] <= 3 * 104`
- Each element in the array appears twice except for one element which appears only once.

### ✅ Summary

> For this problem, the optimal solution uses the XOR operation.
Since every number appears twice except one, XOR naturally cancels out the duplicates and leaves us with the unique with the unique number.
It runs in O(n) time and uses only constant space, satisfying the constraints.
Although HashMap is an easy alternative, it fails the space requirements. 
so XOR is the best choice here.
> 

## First Approach: Bitwise XOR

```java
public int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}
```

- Time : O(n)
- Space : O(1)

### 🔎 Learning Points from the Solution

1. **XOR is powerful for paired canceling problems**
    - When every number appears twice except one, XOR elegantly solves the problem.
    - You don’t need to store anything - the result accumulates directly.
