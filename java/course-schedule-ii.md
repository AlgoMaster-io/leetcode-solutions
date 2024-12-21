# [Leetcode 210: Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

## Table of Contents
- [Approach 1: DFS-Based Topological Sorting](#approach-1-dfs-based-topological-sorting)
- [Approach 2: Kahn’s Algorithm (BFS-Based Topological Sorting)](#approach-2-kahns-algorithm-bfs-based-topological-sorting)

---

## Approach 1: DFS-Based Topological Sorting

The problem of finding an order of courses to take based on prerequisites can be represented as finding a topological sort of a directed graph. Here, each course is a node, and each prerequisite relationship is a directed edge between two nodes.

### Intuition:

1. **Graph Representation**: We represent our courses as nodes and prerequisites as directed edges between these nodes.
2. **DFS for Cycle Detection and Ordering**: Perform DFS to detect cycles in the graph and to help build the ordering of the courses. If we detect a cycle, it's impossible to complete all courses.
3. **Topological Sort**: As we exit the DFS for a node, it's added to the ordering stack/list, ensuring that prerequisites come first before the courses that depend on them.

### Steps:

- Construct a graph using adjacency list representation.
- Use a `visited` list to track the visit status of nodes: unvisited, visiting, or visited.
- Use a stack to assist in constructing the topological order.

### Code:

```java
import java.util.*;

public class CourseScheduleII {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // Build adjacency list representation of the graph
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            graph.add(new ArrayList<>());
        }
        for (int[] prerequisite : prerequisites) {
            graph.get(prerequisite[1]).add(prerequisite[0]);
        }

        // Initialize visit tracking array
        int[] visited = new int[numCourses];
        Stack<Integer> stack = new Stack<>();

        // Perform DFS from each node
        for (int i = 0; i < numCourses; i++) {
            if (dfs(i, graph, visited, stack)) {
                return new int[0]; // Cycle detected, return an empty array
            }
        }
        
        // Build the result from the stack
        int[] result = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            result[i] = stack.pop();
        }
        return result;
    }

    private boolean dfs(int node, List<List<Integer>> graph, int[] visited, Stack<Integer> stack) {
        // Cycle detected
        if (visited[node] == 1) return true;
        // Already visited, no cycle from this node
        if (visited[node] == 2) return false;

        // Mark the node as visiting
        visited[node] = 1;
        
        // Visit all the adjacent nodes
        for (int adj : graph.get(node)) {
            if (dfs(adj, graph, visited, stack)) {
                return true;
            }
        }
        
        // Mark the node as visited
        visited[node] = 2;
        stack.push(node);
        return false;
    }
}
```

### Complexity Analysis

- **Time Complexity**: O(V + E), where V is the number of courses and E is the number of prerequisite pairs.
- **Space Complexity**: O(V + E) for the graph and recursion stack.

---

## Approach 2: Kahn’s Algorithm (BFS-Based Topological Sorting)

### Intuition:

1. **In-Degree Calculation**: Calculate the in-degree for each node (number of prerequisites for each course).
2. **Queue Processing**: Use a queue to process nodes with in-degree of 0 (no prerequisites).
3. **Order Construction**: Gradually build the order of courses by removing nodes from the queue and adjusting in-degrees, adding new nodes with in-degree 0 to the queue.

### Steps:

- Construct the graph and compute in-degrees for each course.
- Use a queue to perform a level-by-level traversal, akin to BFS.
- Track the order and check if all nodes (courses) are processed.

### Code:

```java
import java.util.*;

public class CourseScheduleII {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        // Create the adjacency list and in-degree array
        List<List<Integer>> graph = new ArrayList<>();
        int[] inDegree = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            graph.add(new ArrayList<>());
        }
        for (int[] prerequisite : prerequisites) {
            graph.get(prerequisite[1]).add(prerequisite[0]);
            inDegree[prerequisite[0]]++;
        }

        // Initialize the queue and add nodes with in-degree of 0
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }

        // List to store the course order
        int[] order = new int[numCourses];
        int index = 0;

        // Process nodes in the queue
        while (!queue.isEmpty()) {
            int current = queue.poll();
            order[index++] = current;
            for (int neighbour : graph.get(current)) {
                inDegree[neighbour]--;
                if (inDegree[neighbour] == 0) {
                    queue.offer(neighbour);
                }
            }
        }

        // If the number of courses ordered is not equal to numCourses, we have a cycle
        if (index != numCourses) {
            return new int[0];
        }

        return order;
    }
}
```

### Complexity Analysis

- **Time Complexity**: O(V + E), where V is the number of courses and E is the number of prerequisite pairs.
- **Space Complexity**: O(V + E) because of the adjacency list and degree array.

