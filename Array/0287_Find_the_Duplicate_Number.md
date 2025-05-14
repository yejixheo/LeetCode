# 287. Find the Duplicate Number

| Property | Details |
|----------|--------|
| **Date Solved** | May 13, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, BinarySearch, TwoPointer |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/find-the-duplicate-number/description/) |

## Problem Description 
Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return *this repeated number*.

You must solve the problem **without** modifying the array `nums` and using only constant extra space.

**Example 1:**

```
Input: nums = [1,3,4,2,2]
Output: 2
```

**Example 2:**

```
Input: nums = [3,1,3,4,2]
Output: 3
```

**Example 3:**

```
Input: nums = [3,3,3,3,3]
Output: 3
```

**Constraints:**

- `1 <= n <= 105`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- All the integers in `nums` appear only **once** except for **precisely one integer** which appears **two or more** times.

**Follow up:**

- How can we prove that at least one duplicate number must exist in `nums`?
- Can you solve the problem in linear runtime complexity?

## First Approach: Brute-Force(❌)

```
class Solution {
    public int findDuplicate(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < nums.length; j++) {
                if (i == j) continue;
                if (nums[i] == nums[j]) return nums[i];
            }
        }

        return -1;
        
    }
}
```

- Time : O(n²)
- Space : O(1)

### 🔎 Learning Points from the Solution

1. **Brute-force comparison leads to TLE(Time Limit Exceeded)**
    - Double-loop check is O(n²).

## Second Approach: Fast-Slow Pointers Approach (Floyd’s Cycle Detection)

- **Step 1 : Initialize Pointers**
- **Step 2 : Detect a cycle**
- **Step 3 : Find the Start of the Cycle (Duplicate Number)**

```java
public int findDuplicate(int[] nums) {
    int slow = nums[0];
    int fast = nums[0];

    // Phase 1: Detect cycle
    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow != fast);

    // Phase 2: Find entrance to cycle
    slow = nums[0];
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }

    return slow;
}
```

## Third Approach: Binary Search

- Use Binary search on values (not indices).
For each mid, count how many numbers are `≤ mid` .
If count > mid → duplicate is in left half.

```java
public int findDuplicate(int[] nums) {
    int left = 1;
    int right = nums.length - 1;

    while (left < right) {
        int mid = left + (right - left) / 2;

        // Count how many numbers are less than or equal to mid
        int count = 0;
        for (int num : nums) {
            if (num <= mid) count++;
        }

        if (count > mid) {
            right = mid;  
        } else {
            left = mid + 1; 
        }
    }

    return left;
}
```
