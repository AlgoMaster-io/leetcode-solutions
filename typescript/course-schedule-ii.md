[Leetcode 210: Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

### Approaches
1. [Basic Approach: DFS Topological Sort](#basic-approach-dfs-topological-sort)
2. [Optimized Approach: Kahn's Algorithm for Topological Sort](#optimized-approach-kahns-algorithm-for-topological-sort)

---

### Basic Approach: DFS Topological Sort

**Intuition:**

The problem is essentially asking to find a valid linear ordering of courses given constraints (prerequisites). This can be visualized as a Directed Acyclic Graph (DAG) problem, where courses are nodes and dependencies are directed edges. Topological sorting can help determine the valid order of courses.

1. Use Depth-First Search (DFS) to explore each course.
2. Detect cycles in the graph to ensure we can complete all courses.
3. Reverse the post-order of finished courses to get the topological order.

**Steps:**

1. Represent the course and prerequisites in a graph using adjacency lists.
2. Use an array to track visited status: not visited (0), visiting (-1), visited (1).
3. Execute DFS to visit each course:
   - If visiting, return true for cycle detection.
   - If visited, skip.
   - Otherwise, mark as visiting and visit all neighbors (prerequisites).
4. If a course has a neighbor visited already as visiting, a cycle is detected.
5. Post-complete each course, mark it as visited.
6. Reverse the post-order track to get the course order if no cycle is found.

```typescript
function findOrder(numCourses: number, prerequisites: number[][]): number[] {
    const adjList: number[][] = Array.from({ length: numCourses }, () => []);
    const visitStatus: number[] = Array(numCourses).fill(0);
    const order: number[] = [];

    // Build adjacency list
    for (let [course, prerequisite] of prerequisites) {
        adjList[prerequisite].push(course);
    }

    // Helper DFS function
    const dfs = (course: number): boolean => {
        if (visitStatus[course] === -1) return true; // cycle detected
        if (visitStatus[course] === 1) return false; // course already processed
        
        visitStatus[course] = -1; // mark as visiting

        for (let nextCourse of adjList[course]) {
            if (dfs(nextCourse)) return true; // cycle detected in prerequisites
        }

        visitStatus[course] = 1; // mark as visited
        order.push(course);
        return false;
    };

    // Attempt to visit each course
    for (let i = 0; i < numCourses; i++) {
        if (visitStatus[i] === 0) {
            if (dfs(i)) return []; // Return empty array if a cycle is detected
        }
    }

    return order.reverse();
}
```

- **Time Complexity:** O(V + E) where V is the number of vertices (courses) and E is the number of edges (prerequisites).
- **Space Complexity:** O(V + E) for storing the graph and recursion stack.

---

### Optimized Approach: Kahn's Algorithm for Topological Sort

**Intuition:**

Kahn's Algorithm is an iterative approach to perform topological sorting by leveraging the indegree of nodes. Nodes (courses) with 0 indegree can be added to the result one by one, reducing indegree of connected nodes.

1. Calculate indegrees for each course.
2. Use a queue to process nodes with 0 indegree.
3. Remove processed nodes from the graph, updating the indegree of their neighbors.
4. Courses that reach a 0 indegree during this process are ready to be processed.

**Steps:**

1. Create an indegree list to count prerequisites for each course.
2. Fill adjacency list from the input.
3. Add all nodes with zero indegree to a queue.
4. While queue is not empty:
   - Pop from queue, add to course order.
   - Iterate through neighbors, reduce their indegree.
   - If neighbor reaches 0 indegree, add to queue.
5. If all courses are processed with no cycle, return order. Otherwise, return empty list.

```typescript
function findOrderKahn(numCourses: number, prerequisites: number[][]): number[] {
    const indegree: number[] = Array(numCourses).fill(0);
    const adjList: number[][] = Array.from({ length: numCourses }, () => []);
    const order: number[] = [];

    // Build adjacency list and indegree array
    for (let [course, prerequisite] of prerequisites) {
        adjList[prerequisite].push(course);
        indegree[course]++;
    }

    const queue: number[] = [];

    // Add all courses with zero indegree to the queue
    for (let i = 0; i < numCourses; i++) {
        if (indegree[i] === 0) queue.push(i);
    }

    // Process courses with zero indegree
    while (queue.length > 0) {
        const course = queue.shift()!;
        order.push(course);

        // Update the indegree of neighboring courses
        for (let nextCourse of adjList[course]) {
            indegree[nextCourse]--;
            if (indegree[nextCourse] === 0) queue.push(nextCourse);
        }
    }

    // If all courses are taken, return the order; otherwise, return empty
    return order.length === numCourses ? order : [];
}
```

- **Time Complexity:** O(V + E), where V is the number of courses and E is the number of prerequisite pairs.
- **Space Complexity:** O(V + E), primarily for the adjacency list and indegree array.

### Conclusion

Both approaches handle finding the course order based on prerequisites, ensuring no cycles (indicating a valid order exists). The DFS approach requires recursion, while Kahn’s algorithm utilizes iteration with a queue, both achieving linear complexity relative to the graph's size.

