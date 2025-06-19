# 17. Letter Combinations of a Phone Number

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 18, 2025 |
| **Difficulty** | Medium |
| **Topic** | String, Backtracking |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/) |

## Problem Description 
Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png)

**Example 1:**

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**Example 2:**

```
Input: digits = ""
Output: []
```

**Example 3:**

```
Input: digits = "2"
Output: ["a","b","c"]
```

**Constraints:**

- `0 <= digits.length <= 4`
- `digits[i]` is a digit in the range `['2', '9']`.

### ✅ Summary

> I used backtracking because it allows us to explore all letter combinations in a depth-first manner. At each digit, we try all mapped characters and build a path incrementally. We backtrack by removing the last character and trying the next one, which ensures all combinations are covered.
> 

## First Approach

```java
class Solution {

    private String[] letters = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", 
    "wxyz"};

    public List<String> letterCombinations(String digits) {
        
        List<String> res = new ArrayList<>();
        if (digits.length() == 0) return res;

        backtrack(digits, 0, new StringBuilder(), res);
        return res;
    }

    private void backtrack(String digits, int level, StringBuilder sb, List<String> res) {

        if (level == digits.length()) {
            res.add(sb.toString());
            return;
        }

        int digit = digits.charAt(level) - '0';
        for (int i = 0; i < letters[digit].length(); i++) {
            sb.append(letters[digit].charAt(i));
            backtrack(digits, level + 1, sb, res);
            sb.deleteCharAt(sb.length() - 1);
        }

    }
}
```
