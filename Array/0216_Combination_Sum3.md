# 216. Combination Sum 3

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 26, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, Backtracking |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/combination-sum-iii/description/) |

## Problem Description 
Find all valid combinations of `k` numbers that sum up to `n` such that the following conditions are true:

- Only numbers `1` through `9` are used.
- Each number is used **at most once**.

Return *a list of all possible valid combinations*. The list must not contain the same combination twice, and the combinations may be returned in any order.

**Example 1:**

```
Input: k = 3, n = 7
Output: [[1,2,4]]
Explanation:
1 + 2 + 4 = 7
There are no other valid combinations.
```

**Example 2:**

```
Input: k = 3, n = 9
Output: [[1,2,6],[1,3,5],[2,3,4]]
Explanation:
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
There are no other valid combinations.
```

**Example 3:**

```
Input: k = 4, n = 1
Output: []
Explanation: There are no valid combinations.
Using 4 different numbers in the range [1,9], the smallest sum we can get is 1+2+3+4 = 10 and since 10 > 1, there are no valid combination.
```

**Constraints:**

- `2 <= k <= 9`
- `1 <= n <= 60`

### ✅ Summary

> I solved this problem using backtracking, starting from 1 to 9. At each step, I track how many numbers are left to pick and how much sum is remaining. I added pruning logic to stop recursion early when the remaining sum becomes negative or when we exceed the number of elements to use. Additionally, I used a quick check before starting to return early if it’s mathematically impossible to reach the target.
> 

## First Approach: Backtracking

```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<>();
        if ((k * (k + 1) / 2) > n) return res;
        backtrack(k, n, 1, res, new ArrayList<>());
        return res;
    }

    private void backtrack(int cnt, int remain, int start, List<List<Integer>> res, List<Integer> tmp) {
        if (cnt == 0 && remain == 0) {
            res.add(new ArrayList<>(tmp));
            return;
        }

        if (cnt == 0 || remain < 0) return;

        for (int i = start; i < 10; i++) {
            tmp.add(i);
            backtrack(cnt - 1, remain - i, i + 1, res, tmp);
            tmp.remove(tmp.size() - 1);
        }

    }
}
```
