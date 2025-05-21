# 128. Longest Consecutive Sequence

| Property | Details |
|----------|--------|
| **Date Solved** | May 21, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, HashTable |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/longest-consecutive-sequence/description/) |

## Problem Description 
Given an unsorted array of integers `nums`, return *the length of the longest consecutive elements sequence.*

You must write an algorithm that runs in `O(n)` time.

**Example 1:**

```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is[1, 2, 3, 4]. Therefore its length is 4.

```

**Example 2:**

```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9

```

**Example 3:**

```
Input: nums = [1,0,1,2]
Output: 3

```

**Constraints:**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

### ✅ Summary

> I used a HashSet to store all numbers and removed each number as I visited it to ensure each number is processed only once.
> 

## First Approach: Hash Set

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        
        int max = 0;

        Set<Integer> numSet = new HashSet<>();
        for (int num : nums) {
            numSet.add(num);
        }

        for (int num : nums) {
            if (!numSet.contains(num)) continue;

            int cnt = 1;
            numSet.remove(num);

            int left = num - 1;
            while (numSet.remove(left)) {
                cnt++;
                left--;
            }

            int right = num + 1;
            while (numSet.remove(right)) {
                cnt++;
                right++;
            }

            max = Math.max(max, cnt);
        }

        return max;
    }
}
```

- Time : O(n)
- Space : O(n)

### 🔎 Learning Points from the Solution

1. **Naive `set.contains()` loop can cause TLE(Time Limited Error)**
    - Without removing visited elements, each sequence can take O(n) → Worst case becomes O(n2)
