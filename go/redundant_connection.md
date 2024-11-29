# 684. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approach 1: Union-Find Algorithm

### Solution
```go
// Time Complexity: O(n * α(n)), where α is the inverse Ackermann function
// Space Complexity: O(n)
package main

func findRedundantConnection(edges [][]int) []int {
    n := len(edges)
    parent := make([]int, n+1)
    rank := make([]int, n+1)

    // Initialize the parent for each node as itself
    for i := 1; i <= n; i++ {
        parent[i] = i
        rank[i] = 1 // Initialize the rank
    }

    for _, edge := range edges {
        // If the two nodes are already connected, this edge is redundant
        if union(edge[0], edge[1], parent, rank) {
            return edge
        }
    }

    return []int{} // If no redundant connection is found
}

func find(node int, parent []int) int {
    // Path compression optimization
    if parent[node] != node {
        parent[node] = find(parent[node], parent)
    }
    return parent[node]
}

func union(u, v int, parent, rank []int) bool {
    rootU := find(u, parent)
    rootV := find(v, parent)

    if rootU == rootV {
        return true // u and v are already connected
    }

    // Union by rank
    if rank[rootU] > rank[rootV] {
        parent[rootV] = rootU
    } else if rank[rootU] < rank[rootV] {
        parent[rootU] = rootV
    } else {
        parent[rootV] = rootU
        rank[rootU]++
    }
    return false
}
```

## Approach 2: DFS to Detect Cycle

### Solution
```go
// Time Complexity: O(n^2), in the worst case for a dense graph
// Space Complexity: O(n)
package main

func findRedundantConnection(edges [][]int) []int {
    n := len(edges)
    graph := make([][]int, n+1)

    // Initialize adjacency list
    for i := 0; i <= n; i++ {
        graph[i] = []int{}
    }

    for _, edge := range edges {
        visited := make([]bool, n+1)
        if hasCycleDFS(graph, edge[0], edge[1], visited) {
            return edge
        }
        graph[edge[0]] = append(graph[edge[0]], edge[1])
        graph[edge[1]] = append(graph[edge[1]], edge[0])
    }
    
    return []int{} // If no redundant connection is found
}

func hasCycleDFS(graph [][]int, source, target int, visited []bool) bool {
    if visited[source] {
        return false
    }

    visited[source] = true

    for _, neighbor := range graph[source] {
        if neighbor == target {
            continue
        }
        if visited[neighbor] || hasCycleDFS(graph, neighbor, target, visited) {
            return true
        }
    }
    return false
}
```

