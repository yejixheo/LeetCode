# 199. Binary Tree Right Side View

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 02, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, DFS, BFS |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/binary-tree-right-side-view/description/) |

## Problem Description 
Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return *the values of the nodes you can see ordered from top to bottom*.

**Example 1:**

**Input:** root = [1,2,3,null,5,null,4]

**Output:** [1,3,4]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/11/24/tmpd5jn43fs-1.png)

**Example 2:**

**Input:** root = [1,2,3,4,null,null,null,5]

**Output:** [1,3,4,5]

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/11/24/tmpkpe40xeh-1.png)

**Example 3:**

**Input:** root = [1,null,3]

**Output:** [1,3]

**Example 4:**

**Input:** root = []

**Output:** []

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

### ✅ Summary

> In my solution, I used a queue to perform a BFS level-order traversal of the binary tree. During each level, I kept track of the index `i` and when `i == level size - 1`, I added that node to the result. This allowed me to capture the rightmost visible node at each level efficiently without additional space or post-processing. Although the queue inserts the left child first, it doesn’t affect correctness because we’re only interested in the last node per level. To improve clarity, one could reverse the enqueue order to reflect the visual right-to-left logic.
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
    public List<Integer> rightSideView(TreeNode root) {
        
        List<Integer> res =  new ArrayList<>();
        
        if (root == null) return res;
        
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);

        while (!q.isEmpty()) {
            int currSize = q.size();

            for (int i = 0; i < currSize; i++) {
                TreeNode curr = q.poll();
                if (i == (currSize - 1)) {
                    res.add(curr.val);
                }

                if (curr.left != null) q.offer(curr.left);
                if (curr.right != null) q.offer(curr.right);
            }
        }

        return res;
    }
}
```

- Time : O(n)
- Space : O(n)

### 🔎 Learning Points from the Solution

1. **Queue ordering may mislead interpretation**
    - Because left child is added before right, it may be counterintuitive for some.

## Second Approach: BFS(Improved version)

```java
public List<Integer> rightSideView(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) return res;

    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            TreeNode curr = q.poll();
            if (i == 0) res.add(curr.val);  

            if (curr.right != null) q.offer(curr.right);
            if (curr.left != null) q.offer(curr.left);
        }
    }

    return res;
}
```

## Third Approach: DFS
```java
public List<Integer> rightSideView(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    dfs(root, 0, res);
    return res;
}

private void dfs(TreeNode node, int depth, List<Integer> res) {
    if (node == null) return;

    if (depth == res.size()) {
        res.add(node.val);  // 레벨당 첫 번째 방문 노드 = 오른쪽 가장자리 노드
    }

    dfs(node.right, depth + 1, res);  // 먼저 오른쪽
    dfs(node.left, depth + 1, res);
}
```
