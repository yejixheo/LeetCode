# 448. Find All Numbers Disappeared in an Array

| Property | Details |
|----------|---------|
| **Date Solved** | May 8, 2025 |
| **Difficulty** | Easy |
| **Topic** | Array, HashTable |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/description/) |

## Problem Description 
Given an array `nums` of `n` integers where `nums[i]` is in the range `[1, n]`, return *an array of all the integers in the range* `[1, n]` *that do not appear in* `nums`.

**Example 1:**

```
Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
```

**Example 2:**

```
Input: nums = [1,1]
Output: [2]
```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 105`
- `1 <= nums[i] <= n`

## First Approach: Boolean Array Presence Marking

```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        int N = nums.length;
        boolean[] visited = new boolean[N + 1];

        for (int num : nums) {
            visited[num] = true;
        }

        List<Integer> answer = new ArrayList<>();
        for (int i = 1; i <= N; i++) {
            if (!visited[i]) answer.add(i);
        }

        return answer;
    }
}
```

- Time : O(n)
- Space : O(n)

### 🔎 Learning Points from the Solution

1. **Extra space requirement may not be ideal**
    - Boolean array requires O(n) additional space
2. **No reuse of input array**
    - A separate visited array increases memory usage unnecessarily

## Second Approach: In-place Negative Marking

- Use the input array itself to mark visited indices by flipping them negative.
- Time : O(n)
- Space : O(1)

```java
public List<Integer> findDisappearedNumbers(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        int index = Math.abs(nums[i]) - 1;
        nums[index] = -Math.abs(nums[index]);
    }

    List<Integer> result = new ArrayList<>();
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] > 0) {
            result.add(i + 1);
        }
    }

    return result;
}
```

## Third Approach: Hash Set Lookup

- Time : O(n)
- Space : O(n)

### ⚠️ Hash Set VS Boolean Array

If the input range is small and known, like 1 to n, I would prefer a boolean array because it offers better time and space performance.
However, if the input contains unknown or irregular values, or if readability is important, a HashSet would be more appropriate.
It really depends on the constraints — for optimization, I go with boolean arrays; for flexibility or clarity, HashSet is the better choice.

| Category | Boolean Array | Hash Set | Winner |
| --- | --- | --- | --- |
| Memory Efficiency | Fixed size, cache-friendly | Dynamic, overhead from hashing | Boolean Array |
| Time Performance | O(1), faster in practice | O(1) average, slightly slower | Boolean Array |
| Input Flexibility | Only works for known, positive range | Works for any numbers(negatives, etc.) | Hash Set |
| Readability | Less intuitive for some | Easy to read (`set.contains(x)`) | Hash Set |
</aside>

```java
public List<Integer> findDisappearedNumbers(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    for (int num : nums) {
        seen.add(num);
    }

    List<Integer> result = new ArrayList<>();
    for (int i = 1; i <= nums.length; i++) {
        if (!seen.contains(i)) {
            result.add(i);
        }
    }

    return result;
}
```
