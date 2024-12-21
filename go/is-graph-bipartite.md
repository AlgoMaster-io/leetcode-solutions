# [Leetcode 785: Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)

## Approaches
- [Approach 1: Depth-First Search (DFS) using Recursion](#approach-1-depth-first-search-dfs-using-recursion)
- [Approach 2: Breadth-First Search (BFS) using Queue](#approach-2-breadth-first-search-bfs-using-queue)
- [Approach 3: Union Find](#approach-3-union-find)

---

## Approach 1: Depth-First Search (DFS) using Recursion

### Intuition:
The idea is to recursively attempt to color the graph using two colors. We can think of these colors as "0" and "1". If we are able to color the graph such that no two adjacent nodes have the same color, then the graph is bipartite.

### Implementation:
```go
func isBipartite(graph [][]int) bool {
    color := make([]int, len(graph))
    for i := range color {
        color[i] = -1 // Use -1 to denote uncolored nodes
    }
    for i := 0; i < len(graph); i++ {
        if color[i] == -1 { // If this node has not been colored
            if !dfs(graph, color, i, 0) { // Try to color it using DFS
                return false
            }
        }
    }
    return true
}

func dfs(graph [][]int, color []int, node, c int) bool {
    if color[node] != -1 {
        return color[node] == c // If node is already colored, check for color consistency
    }
    color[node] = c // Color the node
    for _, neighbor := range graph[node] {
        if !dfs(graph, color, neighbor, 1-c) { // Attempt to color neighbors with the opposite color
            return false
        }
    }
    return true
}
```
### Complexity Analysis:
- **Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges, because we are potentially visiting every node and edge once during DFS.
- **Space Complexity:** O(V) for the recursive stack and the color array.

---

## Approach 2: Breadth-First Search (BFS) using Queue

### Intuition:
Similar to DFS, but instead, we use BFS to try and color the graph. This might be more intuitive as it works level by level (distance from starting node).

### Implementation:
```go
func isBipartite(graph [][]int) bool {
    color := make([]int, len(graph))
    for i := range color {
        color[i] = -1 // Initialize all nodes as uncolored
    }
    for i := 0; i < len(graph); i++ {
        if color[i] == -1 {
            queue := []int{i}
            color[i] = 0 // Start coloring with color "0"
            for len(queue) > 0 {
                node := queue[0]
                queue = queue[1:] // Dequeue
                for _, neighbor := range graph[node] {
                    if color[neighbor] == -1 {
                        color[neighbor] = 1 - color[node] // Color neighbor with opposite color
                        queue = append(queue, neighbor) // Enqueue
                    } else if color[neighbor] == color[node] {
                        return false // If a neighbor has the same color, it's not bipartite
                    }
                }
            }
        }
    }
    return true
}
```
### Complexity Analysis:
- **Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges.
- **Space Complexity:** O(V), due to the color array and queue used in BFS.

---

## Approach 3: Union Find

### Intuition:
The Union-Find approach groups nodes into disjoint sets and attempts to partition the graph into two separate sets. If we find an edge that connects nodes in the same set, the graph is not bipartite.

### Implementation:
```go
func isBipartite(graph [][]int) bool {
    parent := make([]int, len(graph))
    for i := range parent {
        parent[i] = i
    }
    
    var find func(x int) int
    find = func(x int) int {
        if parent[x] != x {
            parent[x] = find(parent[x])
        }
        return parent[x]
    }
    
    union := func(x, y int) {
        parent[find(x)] = find(y)
    }
    
    for i, nodes := range graph {
        for _, node := range nodes {
            if find(i) == find(node) {
                return false
            }
            // Union neighbors with a default node to differentiate sets
            union(nodes[0], node)
        }
    }
    return true
}
```
### Complexity Analysis:
- **Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges.
- **Space Complexity:** O(V), primarily due to the parent array tracking sets.

Each of these approaches offers a viable solution to determining if the input graph is bipartite, with space and time efficiency varying based on the intricacy of the graph's connectivity.

