# [Leetcode 797: All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approaches:
1. [Depth-First Search (DFS) with Backtracking](#approach-1-depth-first-search-dfs-with-backtracking)
2. [Breadth-First Search (BFS)](#approach-2-breadth-first-search-bfs)

### Approach 1: Depth-First Search (DFS) with Backtracking

Intuition:
- The problem can be represented as finding all paths from the first node (0) to the last node (n-1) in a Directed Acyclic Graph (DAG) presented in the form of an adjacency list.
- We can use DFS to explore all possible paths from the source node, backtracking when reaching nodes with no further children or reaching the target.
- This approach ensures we explore all paths by recursively visiting each node and storing the path as we go. When we reach the target node, we add the path to our result set.

```go
func allPathsSourceTarget(graph [][]int) [][]int {
    // Result to keep all paths
    var result [][]int
    // Temporary list to store the current traversal path
    var path []int

    // Helper function to perform DFS traversal
    var dfs func(node int)
    dfs = func(node int) {
        // Append current node to path
        path = append(path, node)
        
        // If the last node is reached, add the path to result
        if node == len(graph)-1 {
            // Make a copy of the current path and add it to the result
            temp := make([]int, len(path))
            copy(temp, path)
            result = append(result, temp)
        } else {
            // Explore each adjacent node
            for _, nextNode := range graph[node] {
                dfs(nextNode)
            }
        }

        // Backtrack: Remove the current node to try other paths
        path = path[:len(path)-1]
    }

    // Start DFS from source node 0
    dfs(0)
    return result
}
```

**Time Complexity:** O(2^N \cdot N) in the worst case, where 'N' is the number of nodes. This is because there can be up to 2^N different paths in a binary tree-like structure, each potentially consuming up to 'N' space when it is recorded in the result.

**Space Complexity:** O(N) for the recursion stack and path storage in the worst case.

### Approach 2: Breadth-First Search (BFS)

Intuition:
- A BFS approach will iteratively explore all nodes at the current level before moving onto the nodes at the next depth level.
- We can use a queue to track current paths, appending new nodes to the paths and considering paths complete when reaching the target node.

```go
func allPathsSourceTarget(graph [][]int) [][]int {
    var result [][]int
    queue := [][]int{{0}} // Start with a path starting at the source node

    for len(queue) > 0 {
        // Take the first path from the front of the queue
        path := queue[0]
        queue = queue[1:]
        lastNode := path[len(path)-1]

        // If the last node is reached
        if lastNode == len(graph)-1 {
            result = append(result, path)
        } else {
            // Add paths extending to all adjacent nodes
            for _, neighbor := range graph[lastNode] {
                newPath := append([]int{}, path...)
                newPath = append(newPath, neighbor)
                queue = append(queue, newPath)
            }
        }
    }

    return result
}
```

**Time Complexity:** O(2^N \cdot N) in the worst case. The explanation is similar to that for DFS; there could be many paths equivalent to the DFS exploration.

**Space Complexity:** O(2^N \cdot N). Here, queue can grow to 2^N paths, each with up to 'N' elements. This corresponds to the storage of paths that can exist simultaneously in the queue.

