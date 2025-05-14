# 268. Missing Number

| Property | Details |
|----------|---------|
| **Date Solved** | May 7, 2025 |
| **Difficulty** | Easy |
| **Topic** | Array, HashTable, Sorting |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/missing-number/description/) |

## Problem Description 
Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return *the only number in the range that is missing from the array.*

**Example 1:**

**Input:** nums = [3,0,1]

**Output:** 2

**Explanation:**

`n = 3` since there are 3 numbers, so all numbers are in the range `[0,3]`. 2 is the missing number in the range since it does not appear in `nums`.

**Example 2:**

**Input:** nums = [0,1]

**Output:** 2

**Explanation:**

`n = 2` since there are 2 numbers, so all numbers are in the range `[0,2]`. 2 is the missing number in the range since it does not appear in `nums`.

**Example 3:**

**Input:** nums = [9,6,4,2,3,5,7,0,1]

**Output:** 8

**Explanation:**

`n = 9` since there are 9 numbers, so all numbers are in the range `[0,9]`. 8 is the missing number in the range since it does not appear in `nums`.

**Constraints:**

- `n == nums.length`
- `1 <= n <= 104`
- `0 <= nums[i] <= n`
- All the numbers of `nums` are **unique**.

## First Approach : Sorting Based (Self-solved)

- Time : O(n log n) / Space : O(1)

```java
class Solution {
    public int missingNumber(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;

        for (int i = 1; i < n; i++) {
            if (nums[i] - nums[i-1] != 1) {
                return i;
            }
        }
        
        if (nums[0] == 0) return n;
        else return 0;
    }
}
```

### 🔎 Learning Points from the Solution

1. **Sorting adds unnecessary overhead** 
- Sorting takes O(n log n) time even though the problem can be solved in O(n).
- Also, this solution depends heavily on the array being sorted.
1. **Special handling makes the code less clean**
- Separate logic is needed to handle missing 0 and missing n.
1. **Poor scalability for extended cases**
- This logic only works when exactly one number is missing.
- If requirements change to handle multiple missing numbers, this approach completely breaks.

## Second Approach : Boolean Array (Solution view)

- Create a boolean array to mark seen numbers.
- The index with false is the missing number.
- Easy to understand, but uses extra space.
- Space usage → not memory efficient.
- Overkill for problems where only O(1) space is required.
- Time : O(n) / Space : O(n)

```java
class Solution {
    public int missingNumber(int[] nums) {
        int N = nums.length;

        boolean[] visited = new boolean[N + 1];
        for (int i = 0; i < N; i++) { 
            visited[nums[i]] = true;
        }

        for (int i = 0; i < N + 1; i++) {
            if (!visited[i]) return i;
        }

        return -1;
    }
}
```

```java
//GPT VERSION

class Solution {
    public int missingNumber(int[] nums) {
        int N = nums.length;
        boolean[] visited = new boolean[N + 1];
        
        for (int num : nums) {
			      visited[num] = true;
        }
        
        for (int i = 0; i <= N; i++) {
		        if (!visited[i]) {
				        return i;
		        }
        }
                
        return -1;
    }
}
```

## Third Approach : Sum Formula (Solution view)

- Simple and easy to implement.
- Time : O(n) / Space : O(1)
- Potential risk of integer overflow in extreme cases.
- Only works when exactly one number is missing.
```java
class Solution {
    public int missingNumber(int[] nums) {
        int N = nums.length;

        int sum = 0;
        for (int num : nums) {
            sum += num;
        }

        return ((N * (N + 1)) / 2) - sum;
    }
}
```

```java
class Solution {
    public int missingNumber(int[] nums) {
        int N = nums.length;
        int expectedSum = N * (N + 1) / 2;
        int actualSum = 0;
        
        for (int num : nums) {
		        actualSum += num;
        }
        
        return expectedSum - actualSum;
    }
}
```
