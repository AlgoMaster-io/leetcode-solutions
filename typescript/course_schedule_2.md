# 210. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

## Approach 1: Topological Sort using DFS

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<Integer>[] adjList = new ArrayList[numCourses];
        boolean[] visited = new boolean[numCourses];
        boolean[] onPath = new boolean[numCourses];
        List<Integer> order = new ArrayList<>();

        for (int i = 0; i < numCourses; i++) {
            adjList[i] = new ArrayList<>();
        }

        for (int[] pre : prerequisites) {
            adjList[pre[1]].add(pre[0]);
        }

        for (int i = 0; i < numCourses; i++) {
            if (!visited[i]) {
                if (!dfs(i, adjList, visited, onPath, order)) {
                    return new int[0];
                }
            }
        }

        int[] result = new int[order.size()];
        for (int i = 0; i < order.size(); i++) {
            result[i] = order.get(order.size() - 1 - i);
        }

        return result;
    }

    private boolean dfs(int node, List<Integer>[] adjList, boolean[] visited, boolean[] onPath, List<Integer> order) {
        if (onPath[node]) return false;
        if (visited[node]) return true;

        visited[node] = true;
        onPath[node] = true;

        for (int nei : adjList[node]) {
            if (!dfs(nei, adjList, visited, onPath, order)) {
                return false;
            }
        }

        onPath[node] = false;
        order.add(node);
        return true;
    }
}
```

typescript
```typescript
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
function findOrder(numCourses: number, prerequisites: number[][]): number[] {
    const adjList: number[][] = Array.from({ length: numCourses }, () => []);
    const visited: boolean[] = Array(numCourses).fill(false);
    const onPath: boolean[] = Array(numCourses).fill(false);
    const order: number[] = [];

    for (const [course, pre] of prerequisites) {
        adjList[pre].push(course);
    }

    for (let i = 0; i < numCourses; i++) {
        if (!visited[i]) {
            if (!dfs(i, adjList, visited, onPath, order)) {
                return [];
            }
        }
    }

    return order.reverse();
}

function dfs(node: number, adjList: number[][], visited: boolean[], onPath: boolean[], order: number[]): boolean {
    if (onPath[node]) return false;
    if (visited[node]) return true;

    visited[node] = true;
    onPath[node] = true;

    for (const nei of adjList[node]) {
        if (!dfs(nei, adjList, visited, onPath, order)) {
            return false;
        }
    }

    onPath[node] = false;
    order.push(node);
    return true;
}
```

## Approach 2: Topological Sort using Kahn’s Algorithm (BFS)

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<Integer>[] adjList = new ArrayList[numCourses];
        int[] indegree = new int[numCourses];
        List<Integer> order = new ArrayList<>();
        Queue<Integer> queue = new LinkedList<>();

        for (int i = 0; i < numCourses; i++) {
            adjList[i] = new ArrayList<>();
        }

        for (int[] pre : prerequisites) {
            adjList[pre[1]].add(pre[0]);
            indegree[pre[0]]++;
        }

        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.offer(i);
            }
        }

        while (!queue.isEmpty()) {
            int current = queue.poll();
            order.add(current);

            for (int neighbor : adjList[current]) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }

        if (order.size() == numCourses) {
            int[] result = new int[order.size()];
            for (int i = 0; i < order.size(); i++) {
                result[i] = order.get(i);
            }
            return result;
        } else {
            return new int[0];
        }
    }
}
```

typescript
```typescript
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
function findOrder(numCourses: number, prerequisites: number[][]): number[] {
    const adjList: number[][] = Array.from({ length: numCourses }, () => []);
    const indegree: number[] = Array(numCourses).fill(0);
    const order: number[] = [];
    const queue: number[] = [];

    for (const [course, pre] of prerequisites) {
        adjList[pre].push(course);
        indegree[course]++;
    }

    for (let i = 0; i < numCourses; i++) {
        if (indegree[i] === 0) {
            queue.push(i);
        }
    }

    while (queue.length) {
        const current = queue.shift()!;
        order.push(current);

        for (const neighbor of adjList[current]) {
            indegree[neighbor]--;
            if (indegree[neighbor] === 0) {
                queue.push(neighbor);
            }
        }
    }

    return order.length === numCourses ? order : [];
}
```

