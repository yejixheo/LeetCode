# 22. Generate Parentheses

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 13, 2025 |
| **Difficulty** | Medium |
| **Topic** | String, Backtracking |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/generate-parentheses/description/) |

## Problem Description 
Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.

**Example 1:**

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2:**

```
Input: n = 1
Output: ["()"]
```

**Constraints:**

- `1 <= n <= 8`

### ✅ Summary

> To solve this problem, I used backtracking to build all valid combinations of parentheses step by step. I kept track of how many open and close parentheses had been added so far. Whenever I had room to add a `(` , I added it and moved forward. Similarly, if the number of `)` was less than `(` , I added a `)` to maintain balance. Instead of creating a new string on each recursive call, I used a mutable `StringBuilder` to reduce overhead and avoid excessive memory usage.
> 

## First Approach

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList<>();
        backtrack(res, 0, 0, new StringBuilder(), n);
        return res;
    }

    private void backtrack (List<String> res, int left, int right, StringBuilder sb, int n) {
        if (sb.length() == n * 2) {
            res.add(sb.toString());
            return;
        }

        if (left < n) {
            backtrack(res, left + 1, right, sb.append("(") , n);
            sb.deleteCharAt(sb.length() - 1);
        }

        if (right < left) {
            backtrack(res, left, right + 1, sb.append(")"), n);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```
