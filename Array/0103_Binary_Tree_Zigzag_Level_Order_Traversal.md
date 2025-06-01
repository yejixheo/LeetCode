# 103. Binary Tree Zigzag Level Order Traversal

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 01, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, DFS, BFS |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/) |

## Problem Description 
Given the `root` of a binary tree, return *the zigzag level order traversal of its nodes' values*. (i.e., from left to right, then right to left for the next level and alternate between).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]
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
- `-100 <= Node.val <= 100`

### ✅ Summary

> In my initial solution, I implemented a level-order traversal using a queue and used a counter to track the level index. If the level was odd-numbered, I reversed the list of node values before adding it to the result. This method introduces performance overhead due to `Collections.reverse()` , which takes O(k) time per level. To optimize, I would switch to using a `Deque` so I can insert values in the correct order during traversal without needing to reverse the list. This would ensure a consistent O(n) time complexity and improve overall efficiency.
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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        int cntLv = 0;

        while (!q.isEmpty()) {
            int qSzie = q.size();
            List<Integer> curLv = new ArrayList<>(qSzie);

            for (int i = 0; i < qSzie; i++) {
                TreeNode curNode = q.poll();
                curLv.add(curNode.val);
                if (curNode.left != null) q.offer(curNode.left);
                if (curNode.right != null) q.offer(curNode.right);
            }

            if (cntLv % 2 != 0) Collections.reverse(curLv);
            res.add(curLv);
            cntLv++;
        }
        return res;
    }
}
```

- Time : O(n²)
- Space : O(n)

### 🔎 Learning Points from the Solution

1. **Performance issue due to `Collections.reverse()`**
    - Reversing a list at every odd level adds O(k) time per level.
    - In the worst case, this results in total O(n²) time complexity.

## Second Approach: BFS(Improved Version.)

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        
        List<List<Integer>> res = new ArrayList<>();
		    if (root == null) return res;
		
		    Queue<TreeNode> q = new LinkedList<>();
		    q.offer(root);
		    boolean leftToRight = true;
		
		    while (!q.isEmpty()) {
		        int size = q.size();
		        Deque<Integer> level = new LinkedList<>();
		
		        for (int i = 0; i < size; i++) {
		            TreeNode node = q.poll();
		            if (leftToRight) level.addLast(node.val);
		            else level.addFirst(node.val);
		
		            if (node.left != null) q.offer(node.left);
		            if (node.right != null) q.offer(node.right);
		        }
		
		        res.add(new ArrayList<>(level));
		        leftToRight = !leftToRight;
		    }
		
		    return res;           
		}
}
```

## Third Approach: DFS

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        
        List<List<Integer>> res = new ArrayList<>();
        dfs(root, 0, res);
        return res;
    
    }

    private void dfs(TreeNode node, int level, List<List<Integer>> res) {
        if (node == null) return;
        if (res.size() <= level) 
            res.add(new LinkedList<>());

        if (level % 2 == 0)
            res.get(level).add(node.val);
        else
            res.get(level).add(0, node.val);
        
        dfs(node.left, level + 1, res);
        dfs(node.right, level + 1, res);
    }

}
```
