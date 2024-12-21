# [Leetcode 834: Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Approaches
- [Approach 1: Brute Force Approach](#approach-1-brute-force-approach)
- [Approach 2: Tree DP Approach](#approach-2-tree-dp-approach)

### Approach 1: Brute Force Approach

**Intuition:**

The naive approach involves calculating the sum of the distances from each node to all other nodes individually. We will traverse the tree using BFS or DFS multiple times, once for each node as the root. While this approach is straightforward, it is inefficient for large trees.

**Steps:**
1. For each node `i`, perform a BFS/DFS starting from `i` to compute the sum of distances from `i` to all other nodes.
2. Store the resultant sum of distances for each node `i` in an array.

**Code:**

```go
package main

import "fmt"

func sumOfDistancesInTree(N int, edges [][]int) []int {
    // Create an adjacency list for the graph.
    graph := make([][]int, N)
    for _, edge := range edges {
        u, v := edge[0], edge[1]
        graph[u] = append(graph[u], v)
        graph[v] = append(graph[v], u)
    }

    result := make([]int, N)
    
    // Function to calculate the sum of distances from node 'start' using DFS.
    var dfs func(int, int, int) int
    dfs = func(node, parent, depth int) int {
        sum := depth
        for _, neighbor := range graph[node] {
            if neighbor != parent {
                sum += dfs(neighbor, node, depth + 1)
            }
        }
        return sum
    }

    // Calculate the sum of distances for each node as the starting point.
    for i := 0; i < N; i++ {
        result[i] = dfs(i, -1, 0)
    }
    
    return result
}

func main() {
    N := 6
    edges := [][]int{{0, 1}, {0, 2}, {2, 3}, {2, 4}, {2, 5}}
    fmt.Println(sumOfDistancesInTree(N, edges)) // Output will be computed.
}
```

**Complexity Analysis:**

- **Time Complexity:** O(N^2), as we are performing a BFS/DFS from each node.
- **Space Complexity:** O(N), for storing the adjacency list.

### Approach 2: Tree DP Approach

**Intuition:**

To optimize, we can use dynamic programming on trees. First, perform a post-order traversal to compute the sum of distances from each node to its descendants and the number of its descendants. Then, use this information in a pre-order traversal to compute the sum of distances for all nodes efficiently.

**Steps:**
1. Perform a post-order DFS traversal to calculate the number of nodes in the subtree rooted at each node and the sum of distances from each node to its descendants.
2. Perform a pre-order DFS traversal to calculate the result for each node using the parent node's results.

**Code:**

```go
package main

import "fmt"

func sumOfDistancesInTree(N int, edges [][]int) []int {
    graph := make([][]int, N)
    for _, edge := range edges {
        u, v := edge[0], edge[1]
        graph[u] = append(graph[u], v)
        graph[v] = append(graph[v], u)
    }

    count := make([]int, N)
    res := make([]int, N)
    
    // Postorder DFS to calculate the number of nodes in subtree and initial res for root
    var postOrder func(int, int)
    postOrder = func(node, parent int) {
        for _, neighbor := range graph[node] {
            if neighbor != parent {
                postOrder(neighbor, node)
                count[node] += count[neighbor]
                res[node] += res[neighbor] + count[neighbor]
            }
        }
        count[node]++
    }

    // Preorder DFS to reuse the distances calculated for the root
    var preOrder func(int, int)
    preOrder = func(node, parent int) {
        for _, neighbor := range graph[node] {
            if neighbor != parent {
                res[neighbor] = res[node] - count[neighbor] + (N - count[neighbor])
                preOrder(neighbor, node)
            }
        }
    }

    // First DFS to calculate initial 'res' for root
    postOrder(0, -1)
    // Second DFS to calculate results based on root's result
    preOrder(0, -1)

    return res
}

func main() {
    N := 6
    edges := [][]int{{0, 1}, {0, 2}, {2, 3}, {2, 4}, {2, 5}}
    fmt.Println(sumOfDistancesInTree(N, edges)) // Output: [8 12 6 10 10 10]
}
```

**Complexity Analysis:**

- **Time Complexity:** O(N), since we traverse each node a constant number of times in both post-order and pre-order traversals.
- **Space Complexity:** O(N), for storing adjacency list and state arrays.


