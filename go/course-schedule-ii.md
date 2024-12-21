# [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

This problem involves finding an order in which courses should be taken given prerequisites. Specifically, you have `numCourses` courses you need to take, labeled from `0` to `numCourses - 1`. Some courses may have prerequisites in the form of pairs, and you need to determine the order in which you can take all courses. If there is no way to finish all courses, return an empty list.

## Table of Contents
1. [Approach 1: DFS with Cyclic Dependency Check](#approach-1)
2. [Approach 2: Topological Sort Using Kahn's Algorithm (BFS)](#approach-2)

## Approach 1: DFS with Cyclic Dependency Check <a name="approach-1"></a>

### Intuition
The problem can be visualized as a graph where each course is a node and the prerequisites are directed edges. To find a valid order, we need to perform a topological sort of the graph. If a cycle is detected during DFS, it means no valid order exists.

### Algorithm
1. Build the adjacency list to represent the graph.
2. Use a DFS approach to traverse the graph.
3. Track visited nodes using a state list to detect cycles (unvisited, visiting, visited).
4. Once a node is fully processed, add it to the result stack.
5. If at any point a cycle is detected, return an empty order.

### Code
```go
func findOrder(numCourses int, prerequisites [][]int) []int {
    // Represent the graph using adjacency list
    graph := make([][]int, numCourses)
    for _, prereq := range prerequisites {
        graph[prereq[1]] = append(graph[prereq[1]], prereq[0])
    }

    // Initialize visited states: 0 = unvisited, 1 = visiting, 2 = visited
    visited := make([]int, numCourses)
    var result []int
    var cycleDetected bool

    // Helper function for DFS
    var dfs func(course int)
    dfs = func(course int) {
        if cycleDetected {
            return
        }
        if visited[course] == 1 { // cycle detected
            cycleDetected = true
            return
        }
        if visited[course] == 2 { // already processed, skip
            return
        }

        visited[course] = 1 // mark as visiting
        for _, nextCourse := range graph[course] {
            dfs(nextCourse)
        }
        visited[course] = 2 // mark as visited
        result = append(result, course) // add to result in postorder
    }

    // Perform DFS on each unvisited node
    for course := 0; course < numCourses; course++ {
        if visited[course] == 0 {
            dfs(course)
        }
    }

    if cycleDetected {
        return []int{}
    }

    // Reverse the result since we need to return in reverse postorder of the DFS
    for i, j := 0, len(result)-1; i < j; i, j = i+1, j-1 {
        result[i], result[j] = result[j], result[i]
    }
    return result
}
```

### Time Complexity
- **Time Complexity**: \(O(V + E)\), where \(V\) is the number of courses (vertices) and \(E\) is the number of prerequisites (edges).
- **Space Complexity**: \(O(V + E)\), for the graph and the recursion stack.

## Approach 2: Topological Sort Using Kahn's Algorithm (BFS) <a name="approach-2"></a>

### Intuition
Kahn's Algorithm is a classic way to perform topological sorting using BFS by removing nodes with zero in-degree and updating the in-degrees of their neighbors.

### Algorithm
1. Build the adjacency list and calculate in-degrees for each course.
2. Initialize a queue with all courses that have zero in-degree.
3. Process each node by removing it (capturing it in the order) and reducing the in-degree of its neighbors.
4. If neighbors end up with zero in-degree, add them to the queue.
5. If all nodes are processed, return the order; otherwise, a cycle exists.

### Code
```go
func findOrderKahns(numCourses int, prerequisites [][]int) []int {
    // Initialize the graph and in-degrees
    graph := make([][]int, numCourses)
    inDegree := make([]int, numCourses)
    for _, prereq := range prerequisites {
        graph[prereq[1]] = append(graph[prereq[1]], prereq[0])
        inDegree[prereq[0]]++
    }

    // Add all nodes with in-degree zero to queue
    q := []int{}
    for i := 0; i < numCourses; i++ {
        if inDegree[i] == 0 {
            q = append(q, i)
        }
    }

    var order []int
    for len(q) > 0 {
        course := q[0]
        q = q[1:]
        order = append(order, course)
        
        // Reduce in-degree for all neighbors
        for _, nextCourse := range graph[course] {
            inDegree[nextCourse]--
            if inDegree[nextCourse] == 0 {
                q = append(q, nextCourse)
            }
        }
    }

    // If not all courses could be placed in the order, return []
    if len(order) != numCourses {
        return []int{}
    }
    return order
}
```

### Time Complexity
- **Time Complexity**: \(O(V + E)\), as each node and edge is processed once.
- **Space Complexity**: \(O(V + E)\), for the graph and the queue.

