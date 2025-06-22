# 1593. Split a String Into the Max Number of Unique Substrings

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 22, 2025 |
| **Difficulty** | Medium |
| **Topic** | Hashtable, String, Backtracking |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/split-a-string-into-the-max-number-of-unique-substrings/description/) |

## Problem Description 
Given a string `s`, return *the maximum number of unique substrings that the given string can be split into*.

You can split string `s` into any list of **non-empty substrings**, where the concatenation of the substrings forms the original string. However, you must split the substrings such that all of them are **unique**.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

```
Input: s = "ababccc"
Output: 5
Explanation: One way to split maximally is ['a', 'b', 'ab', 'c', 'cc']. Splitting like ['a', 'b', 'a', 'b', 'c', 'cc'] is not valid as you have 'a' and 'b' multiple times.
```

**Example 2:**

```
Input: s = "aba"
Output: 2
Explanation: One way to split maximally is ['a', 'ba'].
```

**Example 3:**

```
Input: s = "aa"
Output: 1
Explanation: It is impossible to split the string any further.
```

**Constraints:**

- `1 <= s.length <= 16`
- `s` contains only lower case English letters.

### ✅ Summary

> In this problem, I used backtracking to explore all possible splits of the string. I maintained a set to ensure each substring is unique.
> 

## First Approach

```java
class Solution {

    private int cnt;

    public int maxUniqueSplit(String s) {
        Set<String> set = new HashSet<>();
        backtrack(cnt, set, s, 0);
        return cnt;
    }

    private void backtrack(int tmp, Set<String> set, String s, int start) {
        if (start == s.length()) {
            cnt = Math.max(cnt, tmp);
            return;
        }

        for (int end = start + 1; end <= s.length(); end++) {
            String sub = s.substring(start, end);
            if (!set.contains(sub)) {
                set.add(sub);
                tmp++;
                backtrack(tmp, set, s, end);
                tmp--;
                set.remove(sub);
            }
        }
    }
}
```
