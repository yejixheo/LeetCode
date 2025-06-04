# 310. Minimum Height Trees

| Property | Details |
|----------|--------|
| **Date Solved** | May 29, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, DFS, BFS |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/minimum-height-trees/description/) |

## Problem Description 
A tree is an undirected graph in which any two vertices are connected by *exactly* one path. In other words, any connected graph without simple cycles is a tree.

Given a tree of `n` nodes labelled from `0` to `n - 1`, and an array of `n - 1` `edges` where `edges[i] = [ai, bi]` indicates that there is an undirected edge between the two nodes `ai` and `bi` in the tree, you can choose any node of the tree as the root. When you select a node `x` as the root, the result tree has height `h`. Among all possible rooted trees, those with minimum height (i.e. `min(h)`)  are called **minimum height trees** (MHTs).

Return *a list of all **MHTs'** root labels*. You can return the answer in **any order**.

The **height** of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/01/e1.jpg)

```
Input: n = 4, edges = [[1,0],[1,2],[1,3]]
Output: [1]
Explanation: As shown, the height of the tree is 1 when the root is the node with label 1 which is the only MHT.

```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/01/e2.jpg)

```
Input: n = 6, edges = [[3,0],[3,1],[3,2],[3,4],[5,4]]
Output: [3,4]

```

**Constraints:**

- `1 <= n <= 2 * 104`
- `edges.length == n - 1`
- `0 <= ai, bi < n`
- `ai != bi`
- All the pairs `(ai, bi)` are distinct.
- The given input is **guaranteed** to be a tree and there will be **no repeated** edges.

### ✅ Summary

> I solved this problem by trimming leaves from the outer layers of the tree using a BFS-like process. Instead of checking each node as a root, I identified the center of the tree. In each round, I removed all current leaves and updated their neighbors. This process continues util 1 or 2 nodes remain, which are the roots of minimum height trees.
> 

## First Approach: BFS-based leaf trimming

```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if (n == 1) return List.of(0);

        List<Set<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new HashSet<>());

        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }

        List<Integer> leaves = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (graph.get(i).size() == 1) leaves.add(i);
        }

        while (n > 2) {
            n -= leaves.size();
            List<Integer> newLeaves = new ArrayList<>();

            for (int leaf : leaves) {
                int neighbor = graph.get(leaf).iterator().next();
                graph.get(neighbor).remove(leaf);
                if(graph.get(neighbor).size() == 1) newLeaves.add(neighbor);
            }

            leaves = newLeaves;
        }

        return leaves;
    }
}
```

- Time : O(n)
- Space : O(n)
