# 102. Binary Tree Level Order Traversal

| Property | Details |
|----------|--------|
| **Date Solved** | May 30, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, DFS, BFS |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/binary-tree-level-order-traversal/) |

## Problem Description 
Given the `root` of a binary tree, return *the level order traversal of its nodes' values*. (i.e., from left to right, level by level).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

**Example 2:**

```
Input: root = [1]
Output: [[1]]
```

**Example 3:**

```
Input: root = []
Output: []
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-1000 <= Node.val <= 1000`

### ✅ Summary

> In this problem, I used BFS traversal and created a custom `Pair` class to track both the node and its level index. This allowed me to directly control which sublist each node value should go into during the traversal. While the approach works well, it adds unnecessary complexity by requiring a separate class and explicit index tracking. I realized that I could simplify the logic by using a queue-based BFS that tracks the number of nodes per level using `queue.size()` , which is more concise and avoids manual index control.
> 

## First Approach: BFS

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    private class Pair {
        TreeNode node;
        int index;

        Pair(TreeNode node, int index) {
            this.node = node;
            this.index = index;
        }
    }

    public List<List<Integer>> levelOrder(TreeNode root) {
        
        List<List<Integer>> res = new ArrayList<>();

        if(root == null) return res;
        
        Queue<Pair> q = new LinkedList<>();
        q.offer(new Pair(root, 0));

        while(!q.isEmpty()) {
            Pair p = q.poll();
            TreeNode curr = p.node;
            int idx = p.index;

            if (res.size() <= idx) {
                res.add(new ArrayList<>());
            }

            res.get(idx).add(curr.val);

            if(curr.left != null) q.offer(new Pair(curr.left, idx + 1));
            if(curr.right != null) q.offer(new Pair(curr.right, idx + 1));
        }

        return res;
    }
}
```

- Time : O(N)
- Space : O(N)

### 🔎 Learning Points from the Solution

1. **BFS with `queue.size()` per level simplifies logic and avoids extra classes.**
    - It eleminates the need for a custom `Pair` class and clearly separates nodes by level.

## Second Approach: BFS (Improved)

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) return res;

    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    while (!q.isEmpty()) {
        int size = q.size();
        List<Integer> level = new ArrayList<>();

        for (int i = 0; i < size; i++) {
            TreeNode curr = q.poll();
            level.add(curr.val);
            if (curr.left != null) q.offer(curr.left);
            if (curr.right != null) q.offer(curr.right);
        }

        res.add(level);
    }

    return res;
}
```

## Third Approach: DFS

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    dfs(root, 0, res);
    return res;
}

private void dfs(TreeNode node, int level, List<List<Integer>> res) {
    if (node == null) return;
    if (res.size() == level) {
        res.add(new ArrayList<>());
    }
    res.get(level).add(node.val);
    dfs(node.left, level + 1, res);
    dfs(node.right, level + 1, res);
}
```
