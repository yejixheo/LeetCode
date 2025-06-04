# 863. All Nodes Distance K in Binary Tree

| Property | Details |
|----------|--------|
| **Date Solved** | Jun 04, 2025 |
| **Difficulty** | Medium |
| **Topic** | Tree, DFS, BFS |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/description/) |

## Problem Description 
Given the `root` of a binary tree, the value of a target node `target`, and an integer `k`, return *an array of the values of all nodes that have a distance* `k` *from the target node.*

You can return the answer in **any order**.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/28/sketch0.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, k = 2
Output: [7,4,1]
Explanation: The nodes that are a distance 2 from the target node (with value 5) have values 7, 4, and 1.
```

**Example 2:**

```
Input: root = [1], target = 1, k = 3
Output: []
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 500]`.
- `0 <= Node.val <= 500`
- All the values `Node.val` are **unique**.
- `target` is the value of one of the nodes in the tree.
- `0 <= k <= 1000`

### ✅ Summary

> This problem was solved using a recursive approach that tracks the distance from the target node upward to its ancestors while adding distant nodes in the opposite subtree.
> 

## First Approach: DFS

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {

    private List<Integer> res = new ArrayList<>();
    private TreeNode target;
    private int k;

    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        
        this.target = target;
        this.k = k;
        findTarget(root);
        
        return res;
    }

    private int findTarget(TreeNode node) {
        if (node == null) return -1;

        if (node == target) {
            subtreeAdd(node, 0);
            return 1;
        }

        int L = findTarget(node.left);
        int R = findTarget(node.right);

        if (L != -1) {
            if (L == k) res.add(node.val);
            subtreeAdd(node.right, L + 1);
            return L + 1;
        }

        if (R != -1) {
            if (R == k) res.add(node.val);
            subtreeAdd(node.left, R + 1);
            return R + 1;
        }

        return -1;
    }

    private void subtreeAdd(TreeNode node, int dist) {
        if (node == null) return;
        if (dist == k) {
            res.add(node.val);
            return;
        }
        subtreeAdd(node.left, dist + 1);
        subtreeAdd(node.right, dist + 1);
    }
}

```

- Time : O(n)
- Space : O(n)

## Second Approach: BFS

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {

    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
       Map<TreeNode, List<TreeNode>> graph = new HashMap<>();
       buildGraph(root, null, graph);

       Queue<TreeNode> q = new LinkedList<>();
       Set<TreeNode> visited = new HashSet<>();
       List<Integer> res = new ArrayList<>();

       q.offer(target);
       visited.add(target);
       int dist = 0;

       while(!q.isEmpty()) {
        if (dist == k) {
            for (TreeNode node : q) res.add(node.val);
            break;
        }
        int size = q.size();
        for (int i = 0; i < size; i++) {
            TreeNode curr = q.poll();
            for (TreeNode next : graph.get(curr)) {
                if (!visited.contains(next)) {
                    visited.add(next);
                    q.offer(next);
                }
            }
        }
        dist++;
       }
       return res;
    }

    private void buildGraph(TreeNode node, TreeNode parent, Map<TreeNode, List<TreeNode>> graph) {
        if (node == null) return;
        graph.putIfAbsent(node, new ArrayList<>());
        if (parent != null) {
            graph.get(node).add(parent);
            graph.get(parent).add(node);
        }
        buildGraph(node.left, node, graph);
        buildGraph(node.right, node, graph);
    }

}

```
