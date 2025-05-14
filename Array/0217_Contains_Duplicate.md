# 217. Contains Duplicate

| Property | Details |
|----------|---------|
| **Date Solved** | May 7, 2025 |
| **Difficulty** | Easy |
| **Topic** | Array, HashTable, Sorting |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/contains-duplicate/description/) |

## Problem Description 
Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**

**Input:** nums = [1,2,3,1]

**Output:** true

**Explanation:**

The element 1 occurs at the indices 0 and 3.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** false

**Explanation:**

All elements are distinct.

**Example 3:**

**Input:** nums = [1,1,1,3,3,4,3,2,4,2]

**Output:** true

**Constraints:**

- `1 <= nums.length <= 105`
- `109 <= nums[i] <= 109`
</aside>

## First Approach - Hash Table (Self-solved)

```java
import java.util.Hashtable;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        Hashtable <Integer, Integer> count = new Hashtable<>();

        for (int i = 0; i < nums.length; i++) {
            if(count.containsKey(nums[i])) {
                return true;
            } else {
                count.put(nums[i], 1);
            }
        }

        return false;
    }
}
```

### 🔎 Learning Points from the Solution

1. **Used Hash Table instead of Hash Set**
- At first, I used a HashMap to track seen values.
- But since I didn’t need key-value pairs - just presence checking - A HashSet was more suitable.
- It’s lighter, more memory-efficient, and matches the problems’ purpose better.
1. **Better to design the logic with clear conditions when using Hash Map**
- If I use a Hash Map, it’s better to track the count of each value rather than just using `containsKey()`.
- By checking if the count reaches 2 or more, the logic becomes clearer and more precise.
- This approach also makes the solution more adaptable if future requirements involve counting frequencies or handling custom duplicate conditions.

### 💡 HashMap? or HashSet?

- HashSet is more optimal for checking duplicates but HashMap could provide flexibility for future use cases, such as finding the most frequent number or handling custom conditions.
- While using a HashMap allows for more flexibility-like counting frequencies- it introduces extra space and complexity that may not be needed.

## Second Approach - Hash Set (Solution view)

```java
import java.util.HashSet;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> seen = new HashSet<>();

        for (int i = 0; i < nums.length; i++) {
            if(seen.contains(nums[i])) {
                return true;
            } else {
                seen.add(nums[i]);
            }
        }

        return false;
    }
}
```

## Third Approach - Hash Map (Solution view)

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        Map<Integer, Integer> seen = new HashMap<>();

        for (int num : nums) {
            seen.put(num, seen.getOrDefault(num, 0) + 1);
            
            if (seen.get(num) >= 2) return true;
        }

        return false;
    }
}
```
