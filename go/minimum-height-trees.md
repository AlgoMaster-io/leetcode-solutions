## [Leetcode Problem 310: Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Topological Sorting](#approach-2-topological-sorting)

---

### Approach 1: Brute Force

**Intuition:**

The brute force approach involves considering each node as a potential root and measuring the height when that node is the root. By calculating the height of the tree for each node, we can identify the nodes with the minimum height. The process is repeated for every node, resulting in a time-consuming solution given the number of operations necessary. However, it demonstrates a clear foundational understanding of the problem.

**Code:**

```go
func findMinHeightTrees(n int, edges [][]int) []int {
    if n == 1 {
        return []int{0}
    }
    // Create an adjacency list
    neighbors := make([][]int, n)
    for _, edge := range edges {
        neighbors[edge[0]] = append(neighbors[edge[0]], edge[1])
        neighbors[edge[1]] = append(neighbors[edge[1]], edge[0])
    }

    minHeight := n
    result := []int{}
    
    // Helper function to calculate tree height from a root
    var getHeight func(root, parent int) int
    getHeight = func(root, parent int) int {
        height := 0
        for _, neighbor := range neighbors[root] {
            if neighbor != parent {
                height = max(height, 1 + getHeight(neighbor, root))
            }
        }
        return height
    }
    
    // Check each node as a potential root
    for i := 0; i < n; i++ {
        height := getHeight(i, -1)
        if height < minHeight {
            minHeight = height
            result = []int{i}
        } else if height == minHeight {
            result = append(result, i)
        }
    }

    return result
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

**Time Complexity:** O(N^2), where N is the number of nodes.
**Space Complexity:** O(N), used for storing adjacency list and recursive stack in worst case.

---

### Approach 2: Topological Sorting

**Intuition:**

The optimal way to solve this problem is based on the idea that any tree center will be one of the minimum height trees. By using a topological-sort-like approach, we iteratively trim the leaves of the tree. A tree's leaf is a node with only one connection. By removing these, we gradually find the center or centers of the tree. A tree can have either one or two centers. Those will be the roots of the Minimum Height Trees.

**Code:**

```go
func findMinHeightTrees(n int, edges [][]int) []int {
    if n == 1 {
        return []int{0}
    }

    // Create adjacency list
    neighbors := make([][]int, n)
    degree := make([]int, n)
    for _, edge := range edges {
        u, v := edge[0], edge[1]
        neighbors[u] = append(neighbors[u], v)
        neighbors[v] = append(neighbors[v], u)
        degree[u]++
        degree[v]++
    }

    // Initialize leaves nodes
    leaves := []int{}
    for i := 0; i < n; i++ {
        if degree[i] == 1 {
            leaves = append(leaves, i)
        }
    }

    // Trim the leaves iteratively
    remainingNodes := n
    for remainingNodes > 2 {
        leavesSize := len(leaves)
        remainingNodes -= leavesSize
        newLeaves := []int{}
        for _, leaf := range leaves {
            for _, neighbor := range neighbors[leaf] {
                degree[neighbor]--
                if degree[neighbor] == 1 {
                    newLeaves = append(newLeaves, neighbor)
                }
            }
        }
        leaves = newLeaves
    }

    return leaves
}
```

**Time Complexity:** O(N), where N is the number of nodes.
**Space Complexity:** O(N), for adjacency list, degree count, and leaves storage.

This approach efficiently finds the centers of the tree in linear time, ensuring an optimal solution for larger graphs.

