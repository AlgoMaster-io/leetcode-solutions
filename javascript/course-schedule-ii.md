# [Leetcode 210: Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

## Approaches
1. [Topological Sort using BFS (Kahn's Algorithm)](#approach-1-topological-sort-using-bfs-kahns-algorithm)
2. [Topological Sort using DFS](#approach-2-topological-sort-using-dfs)

## Approach 1: Topological Sort using BFS (Kahn's Algorithm)

### Intuition
The problem is essentially about detecting a valid order of finishing courses which forms a Directed Acyclic Graph (DAG). Kahn's algorithm for topological sorting makes use of indegrees (number of dependencies) of each node to determine the order.

1. Create a graph representation using adjacency list.
2. Calculate indegree (number of prerequisites) for each course.
3. Use a queue to track courses with no prerequisites (indegree == 0).
4. Process the queue and build the output order while reducing the indegree of connected nodes (courses that depend on the current course).
5. If all courses can be ordered, return the order, otherwise return an empty array (cycle detected).

### Code

```javascript
var findOrder = function(numCourses, prerequisites) {
    // Initialize the adjacency list with an empty array for each course
    let graph = Array.from({ length: numCourses }, () => []);
    
    // Initialize indegree array for each course as 0
    let indegree = new Array(numCourses).fill(0);
    
    // Build the graph and fill the indegree of each course
    for (let [course, pre] of prerequisites) {
        graph[pre].push(course); // Directed edge from pre to course
        indegree[course]++; // Increment indegree for this course
    }
    
    // Initialize a queue with all courses having an indegree of 0
    let queue = [];
    for (let i = 0; i < numCourses; i++) {
        if (indegree[i] === 0) {
            queue.push(i);
        }
    }
    
    // Initialize an array to store the order of courses
    let order = [];
    
    while (queue.length) {
        let current = queue.shift(); // Get a course with no dependencies
        order.push(current); // Include it in the order
        
        // Decrease the indegree of its neighboring nodes
        for (let neighbor of graph[current]) {
            indegree[neighbor]--;
            if (indegree[neighbor] === 0) {
                // If indegree is 0, add to the queue to process
                queue.push(neighbor);
            }
        }
    }
    
    // Check if the order includes all courses
    return order.length === numCourses ? order : [];
};
```

### Complexity
- **Time Complexity:** O(V + E), where V is the number of courses, and E is the number of prerequisites. We visit each node once and process each edge once.
- **Space Complexity:** O(V + E) for the adjacency list and the queue.

## Approach 2: Topological Sort using DFS

### Intuition
Another way to perform topological sorting is using Depth First Search. Here, we try to explore as far as possible along each branch before backtracking, marking nodes as visited, and building the topological order through post-order traversal.

1. Create a graph representation using adjacency list.
2. Use a status array to track visited courses to detect cycles and already processed nodes.
3. Perform a DFS to check each course. If visiting a course, mark it as visiting.
4. After visiting all its neighbors, mark it as visited and add to order.
5. If a cycle is detected, return an empty array.
6. The order is reversed post-processing to get the correct order.

### Code

```javascript
var findOrder = function(numCourses, prerequisites) {
    // Initialize the adjacency list
    let graph = Array.from({ length: numCourses }, () => []);
    
    // Build the graph
    for (let [course, pre] of prerequisites) {
        graph[pre].push(course);
    }
    
    // Status array: 0 - unvisited, 1 - visiting, 2 - visited
    let status = new Array(numCourses).fill(0);
    // Stack to store the post-order
    let stack = [];
    let isPossible = true;

    // Define the DFS function
    const dfs = (node) => {
        if (!isPossible) return;
        
        status[node] = 1; // Mark node as visiting

        // Traverse all dependencies (neighbors)
        for (let neighbor of graph[node]) {
            if (status[neighbor] === 0) {
                dfs(neighbor); // Visit unvisited node
            } else if (status[neighbor] === 1) {
                isPossible = false; // Cycle detected
            }
        }
        
        status[node] = 2; // Mark node as visited
        stack.push(node); // Add to stack in reverse order
    };

    for (let i = 0; i < numCourses; i++) {
        if (status[i] === 0) {
            dfs(i);
        }
    }
    
    return isPossible ? stack.reverse() : [];
};
```

### Complexity
- **Time Complexity:** O(V + E), where V is the number of courses, and E is the number of prerequisites. We essentially visit all nodes and edges once.
- **Space Complexity:** O(V + E) for the graph representation and the recursion stack.

Both approaches are efficient for solving the problem of deriving a topological order from course prerequisites and can be chosen based on preference for BFS or DFS style coding.

