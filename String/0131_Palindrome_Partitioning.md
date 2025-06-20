# 131. Palindrome Partitioning

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 20, 2025 |
| **Difficulty** | Medium |
| **Topic** | String, Backtracking |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/palindrome-partitioning/description/) |

## Problem Description 
Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return *all possible palindrome partitioning of* `s`.

**Example 1:**

```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

**Example 2:**

```
Input: s = "a"
Output: [["a"]]
```

**Constraints:**

- `1 <= s.length <= 16`
- `s` contains only lowercase English letters.

## First Approach: Backtracking

```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> res = new ArrayList<>();
        backtrack(s, 0, new ArrayList<>(), res);
        return res;
    }

    private void backtrack(String s, int start, List<String> tmp, List<List<String>> res) {
        if (s.length() == start) {
            res.add(new ArrayList<>(tmp));
            return;
        }

        for (int end = start + 1; end <= s.length(); end++) {
            String sub = s.substring(start, end);
            if (isPalindrome(sub)) {
                tmp.add(sub);
                backtrack(s, end, tmp, res);
                tmp.remove(tmp.size() - 1);
            }
        }
    }

    private boolean isPalindrome(String str) {
        int st = 0, ed = str.length() - 1;
        while (st < ed) {
            if (str.charAt(st++) != str.charAt(ed--)) return false;
        }
        return true;
    }
}
```
