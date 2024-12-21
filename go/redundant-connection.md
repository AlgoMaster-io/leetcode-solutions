[Leetcode Problem 684: Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approaches
1. [Union-Find (Disjoint Set)](#approach-1-union-find-disjoint-set)

---

### Approach 1: Union-Find (Disjoint Set)

#### Intuition
The problem can be effectively solved using the Union-Find (or Disjoint Set Union, DSU) data structure, which is designed to efficiently connect elements together and determine the connected components. The key requirement here is to detect the redundant connection that forms a cycle.

Each edge between nodes is considered one by one. We perform union operations on the nodes of each edge, expecting that if these nodes are already connected (i.e., they have the same root), adding this edge would form a cycle. Hence, this edge is redundant.

#### Detailed Steps
1. **Initialize the Disjoint Set**: Create a parent array where each node is its own parent indicating that initially, each node is its own connected component.

2. **Find with Path Compression**: Implement the `find` function that, while finding the root of a node, compresses the path, effectively flattening the structure to keep tree height minimal.

3. **Union with Union by Rank**: Implement the `union` function to connect two nodes using their roots, ensuring that the tree's minimal rank is preserved (the smaller tree is attached below the bigger one).

4. **Detect and Return Redundant Edge**: For each edge in the graph, if the nodes of the edge are already connected (i.e., they have the same root), this edge is detected as the redundant edge. Otherwise, perform a union on these nodes.

#### Time Complexity
- **O(N)**: We process each edge once, and each union or find operation is practically a constant time operation due to path compression and union by rank.

#### Space Complexity
- **O(N)**: Space for the parent and rank arrays.

```go
func findRedundantConnection(edges [][]int) []int {
    n := len(edges)
    parent := make([]int, n+1)
    rank := make([]int, n+1)
    
    // Initialize each node's parent to itself
    for i := 0; i <= n; i++ {
        parent[i] = i
    }
    
    // The find function with path compression
    var find func(x int) int
    find = func(x int) int {
        if parent[x] != x {
            parent[x] = find(parent[x]) // path compression step
        }
        return parent[x]
    }
    
    // The union function with union by rank
    union := func(x, y int) bool {
        rootX := find(x)
        rootY := find(y)
        
        if rootX == rootY {
            return false // cycle detected
        }
        
        if rank[rootX] > rank[rootY] {
            parent[rootY] = rootX
        } else if rank[rootX] < rank[rootY] {
            parent[rootX] = rootY
        } else {
            parent[rootY] = rootX
            rank[rootX]++
        }
        
        return true
    }
    
    // Iterate through each edge and perform union-find operations
    for _, edge := range edges {
        if !union(edge[0], edge[1]) {
            return edge // this edge causes a cycle
        }
    }
    
    return []int{}
}
```

In this solution, we have effectively used the Union-Find operation to track the connected components of a dynamic graph. When a union operation attempts to unite two nodes already in the same component, it means adding the current edge would form a cycle, which is why it can be considered redundant. This direct approach efficiently finds the redundant connection.

