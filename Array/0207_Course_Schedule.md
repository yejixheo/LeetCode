# 207. Course Schedule

| Property | Details |
|----------|--------|
| **Date Solved** | May 26, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, DFS, BFS |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/course-schedule/description/) |

## Problem Description 
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take.
To take course 1 you should have finished course 0. So it is possible.

```

**Example 2:**

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take.
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

```

**Constraints:**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- All the pairs prerequisites[i] are **unique**.

### ✅ Interview Summary

> I understood that the problem is about detecting cycles in a directed graph. If there is a cycle in the prerequisites, it’s impossible to complete all courses. DFS uses a three-state system to track visiting status and detect cycles.
> 

## First Approach: DFS

- Track the visit status of each node using a `state[]` array

```java
class Solution {

    public boolean canFinish(int numCourses, int[][] prerequisites) {

        List<Integer>[] graph = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) graph[i] = new ArrayList<>();

        for (int[] pre : prerequisites) {
            graph[pre[1]].add(pre[0]);
        }

        int[] state = new int[numCourses];

        for (int i = 0; i < numCourses; i++) {
            if (!dfs(i, graph, state)) return false;
        }

        return true;
    }

    private boolean dfs(int node, List<Integer>[] graph, int[] state) {
        if (state[node] == 1) return false;
        if (state[node] == 2) return true;

        state[node] = 1;
        for (int next : graph[node]) {
            if (!dfs(next, graph, state)) return false;
        } 
        state[node] = 2;
        return true;
    }
}
```

## Second Approach: BFS

- Build an in-degree array and adjacency list.
- Remove all nodes with in-degree 0 and reduce the in-degree of their neighbors.
- Repeat until all nodes are processed.
```java
class Solution {

    public boolean canFinish(int numCourses, int[][] prerequisites) {

        int[] inDegree = new int[numCourses];
        List<Integer>[] graph = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) graph[i] = new ArrayList<>();

        for (int[] pre : prerequisites) {
            graph[pre[1]].add(pre[0]);
            inDegree[pre[0]]++;
        }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) q.offer(i);
        }

        int cnt = 0;
        while (!q.isEmpty()) {
            int course = q.poll();
            cnt++;
            for (int next : graph[course]) {
                if (--inDegree[next] == 0) q.offer(next);
            }
        }

        return cnt == numCourses;

    }
}
```
