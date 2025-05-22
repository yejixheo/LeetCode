# 41. First Missing Positive

| Property | Details |
|----------|--------|
| **Date Solved** | May 22, 2025 |
| **Difficulty** | Hard |
| **Topic** | Array |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/first-missing-positive/description/) |

## Problem Description 
Given an unsorted integer array `nums`. Return the *smallest positive integer* that is *not present* in `nums`.

You must implement an algorithm that runs in `O(n)` time and uses `O(1)` auxiliary space.

**Example 1:**

```
Input: nums = [1,2,0]
Output: 3
Explanation: The numbers in the range [1,2] are all in the array.
```

**Example 2:**

```
Input: nums = [3,4,-1,1]
Output: 2
Explanation: 1 is in the array but 2 is missing.
```

**Example 3:**

```
Input: nums = [7,8,9,11,12]
Output: 1
Explanation: The smallest positive integer 1 is missing.
```

**Constraints:**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`

### ✅ Summary

> I firstly used an in-place marking technique to flag indices based on positive values. However, my version involved custom handling for zeroes and edge values like `nums.length`, which made the code harder to understand and test. A cleaner approach is to Improve this logic using index negation marking, where each valid number `x` maps to index `x - 1` and its presence is marked by making that position negative. Alternatively, an in-place index swap approach can also be used, where each number `x` is moved to index `x - 1` to create a direct mapping.

## First Approach: In-place Index Marking

```java
class Solution {
    public int firstMissingPositive(int[] nums) {

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] < 0) nums[i] = 0;
        }

        int min = nums.length;

        for (int i = 0; i < nums.length; i++) {
            int curr = Math.abs(nums[i]);

            if (curr == nums.length) min++;

            else if (curr > 0 && curr < nums.length) {
                if (nums[curr] == 0) nums[curr] = -(nums.length + 1);
                else nums[curr] = Math.abs(nums[curr]) * -1;
            }
        }

        for (int i = 1; i < nums.length; i++) {
            if (nums[i] >= 0) {
                min = Math.min(i, min);
            }
        }

        return min;
    }
}
```

- Time : O(n)
- Space : O(1)

### 🔎 Learning Points from the Solution

1. **Zero-handling with special marker adds complexity**
    - Using `nums[curr] == 0` and setting to `-(n + 1)` is hard to follow.
2. **Tracking `nums.length` separately is less elegant.**
    - Updating `min++` feels ad-hoc and lacks clarity.
3. **Standard index negation approach is more readable**
    - Mapping `num -> index` and negating `nums[num - 1]`.

## Second Approach: Index Negation Marking

- Use the input array as a presence tracker : For every value `x` in `[1, n]`, mark `nums[x - 1]` as negative.

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            if (nums[i] <= 0) nums[i] = n + 1; //Replace non-positive numbers with n + 1
        }

        for (int i = 0; i < n; i++) {
            int curr = Math.abs(nums[i]);
            if (curr <= n) {
                nums[curr - 1] = -Math.abs(nums[curr - 1]);
            }
        }

        for (int i = 0; i < n; i++) {
            if (nums[i] > 0) return i + 1;
        }

        return n + 1;

    }
}
```

## Third Approach: In-place Swap to Correct Index

- Place value `x` at index `x - 1` if it’s in the range `[1, n]` .

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                int tmp = nums[i];
                nums[i] = nums[tmp - 1];
                nums[tmp - 1] = tmp;
            }
        }

        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) return i + 1;
        }

        return n + 1;
    }
}
```
