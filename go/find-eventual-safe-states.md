# [Leetcode 802: Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

## Approaches
- [Approach 1: Depth-First Search with Cycle Detection](#approach-1-depth-first-search-with-cycle-detection)
- [Approach 2: Reverse Graph and Topological Sort](#approach-2-reverse-graph-and-topological-sort)

## Approach 1: Depth-First Search with Cycle Detection

### Intuition
The problem can be thought of as a graph problem where we need to detect cycles and identify "safe" nodes. A safe node is one that cannot reach a cycle in the directed graph. One classical way to identify nodes in a cycle is to use DFS with cycle detection.

### Steps
1. Use a color-marking technique during DFS: 
   - Mark unvisited nodes with color `0`.
   - Nodes being processed with color `1`.
   - Completely processed (safe) nodes with color `2`.
2. During DFS:
   - If we visit a node already marked with `1`, a cycle is detected, marking this path as unsafe.
   - Mark nodes fully processed (`2`) as safe.
3. Collect and return all nodes marked as safe.

### Time Complexity
- O(V + E), where V is the number of vertices and E is the number of edges.

### Space Complexity
- O(V) for the recursive call stack and to store colors.

```go
func eventualSafeNodes(graph [][]int) []int {
    n := len(graph)
    color := make([]int, n)
    var result []int
    
    var dfs func(node int) bool
    dfs = func(node int) bool {
        if color[node] > 0 {
            return color[node] == 2
        }
        
        color[node] = 1 // mark node as processing
        for _, neighbor := range graph[node] {
            if color[neighbor] == 2 { 
                continue
            }
            if color[neighbor] == 1 || !dfs(neighbor) {
                return false
            }
        }
        
        color[node] = 2 // mark node as safe once it is fully processed
        return true
    }

    for i := 0; i < n; i++ {
        if dfs(i) {
            result = append(result, i)
        }
    }
    
    return result
}
```

## Approach 2: Reverse Graph and Topological Sort

### Intuition
Instead of directly working with the given graph, reverse the direction of all edges. Then, nodes that eventually can reach no other nodes (like terminal nodes) are safe. Use a topological sort-like approach with zero out-degree nodes.

### Steps
1. Reverse the graph's edges.
2. Track all zero out-degree nodes.
3. Use a queue to perform a BFS to gather all safe nodes - nodes which reduce the out-degree counter to zero while traversing the graph.

### Time Complexity
- O(V + E), where V is the number of vertices and E is the number of edges.

### Space Complexity
- O(V + E) for storing the reversed graph and queue management.

```go
func eventualSafeNodes2(graph [][]int) []int {
    n := len(graph)
    reverseGraph := make([][]int, n)
    outDegree := make([]int, n)
    
    // Build reverse graph
    for node, neighbors := range graph {
        for _, neighbor := range neighbors {
            reverseGraph[neighbor] = append(reverseGraph[neighbor], node)
        }
        outDegree[node] = len(neighbors)
    }
    
    queue := []int{}
    for i := 0; i < n; i++ {
        // Initial nodes with no outgoing edges are candidates for safe nodes
        if outDegree[i] == 0 {
            queue = append(queue, i)
        }
    }
    
    // BFS to gather all safe nodes
    result := []int{}
    for len(queue) > 0 {
        cur := queue[0]
        queue = queue[1:]
        
        // Record nodes as safe
        result = append(result, cur)
        for _, prev := range reverseGraph[cur] {
            outDegree[prev]--
            if outDegree[prev] == 0 {
                queue = append(queue, prev)
            }
        }
    }
    
    sort.Ints(result) // since the output needs to be sorted
    return result
}
```


