# 238. Product of Array Except Self

| Property | Details |
|----------|--------|
| **Date Solved** | May 12, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/product-of-array-except-self/description/) |

## Problem Description
Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

**Example 2:**

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

**Constraints:**

- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- The input is generated such that `answer[i]` is **guaranteed** to fit in a **32-bit** integer.

**Follow up:** Can you solve the problem in `O(1)` extra space complexity? (The output array **does not** count as extra space for space complexity analysis.)

### ✅ Summary

> I used a prefix and suffix array to precompute the product of all numbers to the left and right of each index.
To avoid handling edge cases separately, I initialized prefix[0] = 1 and suffix[N-1] = 1, which allowed me to compute the final result using a single loop (answer[i] = prefix[i] * suffix[i].
This makes the code clean and scalable.
If space optimization was required, I could reduce it to O(1) using the in-place suffix accumulation method.
> 

## First Approach: Prefix & Suffix Arrays

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int N = nums.length;
         
        int[] prefix = new int[N];
        int[] suffix = new int[N];
        int[] answer = new int[N];

        prefix[0] = nums[0];
        suffix[N - 1] = nums[N - 1];         

        for (int i = 1; i < N - 1; i++) {
            prefix[i] = nums[i] * prefix[i - 1];
            suffix[N - 1 - i] = nums[N - 1 - i] * suffix[N - i];
        }

        answer[0] = suffix[1];
        answer[N - 1] = prefix[N - 2];
        for(int i = 1; i < N - 1; i++) {
            answer[i] = prefix[i - 1] * suffix[i + 1];
        }

        return answer;
    }
}
```

### Clean Version (LeetCode Solution)

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int pre[] = new int[n];
        int suff[] = new int[n];
        pre[0] = 1;
        suff[n - 1] = 1;
        
        for(int i = 1; i < n; i++) {
            pre[i] = pre[i - 1] * nums[i - 1];
        }
        for(int i = n - 2; i >= 0; i--) {
            suff[i] = suff[i + 1] * nums[i + 1];
        }
        
        int ans[] = new int[n];
        for(int i = 0; i < n; i++) {
            ans[i] = pre[i] * suff[i];
        }
        return ans;
    }
```

- Time : O(n)
- Space : O(n)

### 🔎 Learning Points from the Solution

1. **Can improve space efficiency**
    - Both prefix and suffix arrays can be avoided by calculating in-place using just the output array and a temp variable.
2. **Avoided hardcoded edge assignments like `answer[0] = suffix[1]`** 
    - Can remove edge condition handling and makes the code cleaner and easier to scale.

## Second Approach: In-place using output array

Store prefix product in the `answer` array first, then multiply suffix product in reverse pass.

```java
public int[] productExceptSelf(int[] nums) {
    int N = nums.length;
    int[] answer = new int[N];

    answer[0] = 1;
    for (int i = 1; i < N; i++) {
        answer[i] = answer[i - 1] * nums[i - 1];  // (times)
    }

    int suffix = 1;
    for (int i = N - 1; i >= 0; i--) {
        answer[i] = answer[i] * suffix;  // (times)
        suffix = suffix * nums[i];      // (times)
    }

    return answer;
}
```

- Time : O(n)
- Space : O(1)
