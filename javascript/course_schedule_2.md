# 210. [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

## Approach 1: Topological Sorting using Kahn's Algorithm

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.*;

public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        int[] inDegree = new int[numCourses];
        List<Integer> order = new ArrayList<>();
        
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new ArrayList<>();
        }

        for (int[] prereq : prerequisites) {
            graph[prereq[1]].add(prereq[0]);
            inDegree[prereq[0]]++;
        }

        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }

        while (!queue.isEmpty()) {
            int course = queue.poll();
            order.add(course);

            for (int neighbor : graph[course]) {
                inDegree[neighbor]--;
                if (inDegree[neighbor] == 0) {
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
        }

        return new int[0];
    }
}
```

### Solution
javascript
```javascript
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
var findOrder = function(numCourses, prerequisites) {
    let graph = Array.from({ length: numCourses }, () => []);
    let inDegree = new Array(numCourses).fill(0);
    let order = [];

    for (let [course, prereq] of prerequisites) {
        graph[prereq].push(course);
        inDegree[course]++;
    }

    let queue = [];
    for (let i = 0; i < numCourses; i++) {
        if (inDegree[i] === 0) {
            queue.push(i);
        }
    }

    while (queue.length > 0) {
        let course = queue.shift();
        order.push(course);

        for (let neighbor of graph[course]) {
            inDegree[neighbor]--;
            if (inDegree[neighbor] === 0) {
                queue.push(neighbor);
            }
        }
    }

    return order.length === numCourses ? order : [];
};
```

## Approach 2: Topological Sorting using DFS

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.*;

public class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new ArrayList<>();
        }

        for (int[] prereq : prerequisites) {
            graph[prereq[1]].add(prereq[0]);
        }

        Deque<Integer> stack = new ArrayDeque<>();
        boolean[] visited = new boolean[numCourses];
        boolean[] onPath = new boolean[numCourses];

        for (int i = 0; i < numCourses; i++) {
            if (!visited[i] && !dfs(graph, i, stack, visited, onPath)) {
                return new int[0];
            }
        }

        int[] result = new int[stack.size()];
        int index = 0;
        while (!stack.isEmpty()) {
            result[index++] = stack.pop();
        }

        return result;
    }

    private boolean dfs(List<Integer>[] graph, int course, Deque<Integer> stack, boolean[] visited, boolean[] onPath) {
        if (onPath[course]) {
            return false;
        }
        if (visited[course]) {
            return true;
        }

        onPath[course] = true;
        visited[course] = true;

        for (int neighbor : graph[course]) {
            if (!dfs(graph, neighbor, stack, visited, onPath)) {
                return false;
            }
        }

        stack.push(course);
        onPath[course] = false;
        return true;
    }
}
```

### Solution
javascript
```javascript
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
var findOrder = function(numCourses, prerequisites) {
    let graph = Array.from({ length: numCourses }, () => []);
    for (let [course, prereq] of prerequisites) {
        graph[prereq].push(course);
    }

    let stack = [];
    let visited = new Array(numCourses).fill(false);
    let onPath = new Array(numCourses).fill(false);

    function dfs(course) {
        if (onPath[course]) return false;
        if (visited[course]) return true;

        onPath[course] = true;
        visited[course] = true;

        for (let neighbor of graph[course]) {
            if (!dfs(neighbor)) return false;
        }

        stack.push(course);
        onPath[course] = false;
        return true;
    }

    for (let i = 0; i < numCourses; i++) {
        if (!visited[i] && !dfs(i)) return [];
    }

    return stack.reverse();
};
```

