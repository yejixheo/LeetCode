# 210. Course Schedule 2

| Property | Details |
|----------|--------|
| **Date Solved** | May 28, 2025 |
| **Difficulty** | Medium |
| **Topic** | Array, DFS, BFS |
| **URL** | [LeetCode Problem](https://leetcode.com/problems/course-schedule-ii/description/) |

## Problem Description 
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return *the ordering of courses you should take to finish all courses*. If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].

```

**Example 2:**

```
Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].

```

**Example 3:**

```
Input: numCourses = 1, prerequisites = []
Output: [0]

```

**Constraints:**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- `ai != bi`
- All the pairs `[ai, bi]` are **distinct**.

### ✅ Summary

> I solved this using BFS-based approach to topological sorting. I first built a directed graph using an adjacency list, where each course points to its dependent courses. then I created `inDegree` array to track how many prerequisites each course has. Courses with no prerequisites are added to a queue, and I process them in order. Each time I process a course, I decrease the in-degree of its children and enqueue them if their in-degree becomes zero. Finally, if I processed all the courses, I return the result list as an array. If not, it means there’s a cycle, so I return an empty array. One thing I realized is that converting the list to an array manually at the end was unnecessarily verbose. I could have maintained an `int[]` from the start and filled it directly while dequeuing elements from the queue.
> 

## First Approach:

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        int[] inDegree = new int[numCourses];
        List<Integer>[] graph = new ArrayList[numCourses];

        for (int i = 0; i < numCourses; i++) {
            graph[i] = new ArrayList<>();
        }

        for (int[] pre : prerequisites) {
            graph[pre[1]].add(pre[0]);
            inDegree[pre[0]]++;
        }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) q.offer(i);
        }

        List<Integer> res = new ArrayList<>();
        while (!q.isEmpty()) {
            int course = q.poll();
            res.add(course);
            for (int next : graph[course]) {
                if (--inDegree[next] == 0) q.offer(next);
            }
        }

        if (res.size() == numCourses) {
            int[] ans = new int[res.size()];
            for (int i = 0; i < res.size(); i++) {
                ans[i] = res.get(i);
            }
            return ans;
        }

        return new int[0];
    }
}
```

- Time : O(V + E)
- Space : O(V + E)

### 🔎 Learning Points from the Solution

1. **Converts `List<Integer` to `int[]` manually, which could be simplified.**

## Improved version.

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) graph.add(new ArrayList<>());

        int[] inDegree = new int[numCourses];
        for (int[] pre : prerequisites) {
            graph.get(pre[1]).add(pre[0]);
            inDegree[pre[0]]++;
        }

        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) queue.offer(i);
        }

        int[] order = new int[numCourses];
        int idx = 0;

        while (!queue.isEmpty()) {
            int cur = queue.poll();
            order[idx++] = cur;

            for (int next : graph.get(cur)) {
                inDegree[next]--;
                if (inDegree[next] == 0) queue.offer(next);
            }
        }

        return idx == numCourses ? order : new int[0];
    }
}
```
