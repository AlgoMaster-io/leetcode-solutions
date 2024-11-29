# 797. [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approach: Depth-First Search (DFS)

### Solution
```go
// Time Complexity: O(2^n * n), where n is the number of nodes in the graph
// Space Complexity: O(2^n * n), for storing all paths in the result
package main

import "fmt"

func allPathsSourceTarget(graph [][]int) [][]int {
    result := [][]int{}
    path := []int{0} // Start from the source node
    dfs(graph, 0, path, &result)
    return result
}

func dfs(graph [][]int, node int, path []int, result *[][]int) {
    if node == len(graph)-1 {
        // If we reach the target node, add the current path to the result
        pathCopy := make([]int, len(path))
        copy(pathCopy, path)
        *result = append(*result, pathCopy)
        return
    }

    for _, neighbor := range graph[node] {
        path = append(path, neighbor) // Add the neighbor to the current path
        dfs(graph, neighbor, path, result) // Recur for the neighbor
        path = path[:len(path)-1] // Backtrack to explore other paths
    }
}

func main() {
    graph := [][]int{{1, 2}, {3}, {3}, {}}
    fmt.Println(allPathsSourceTarget(graph)) // Output: [[0 1 3] [0 2 3]]
}
```

