# 834. [Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Approach 1: DFS to Calculate Subtree Sizes and Initial Distance

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
import "fmt"

func sumOfDistancesInTree(N int, edges [][]int) []int {
    tree := make([][]int, N)
    answer := make([]int, N)
    count := make([]int, N)

    // Build the tree as an adjacency list
    for _, edge := range edges {
        tree[edge[0]] = append(tree[edge[0]], edge[1])
        tree[edge[1]] = append(tree[edge[1]], edge[0])
    }

    // Post-order DFS to calculate count and initial distances
    var postOrder func(int, int)
    postOrder = func(node int, parent int) {
        count[node] = 1
        for _, neighbor := range tree[node] {
            if neighbor == parent {
                continue
            }
            postOrder(neighbor, node)
            count[node] += count[neighbor]
            answer[node] += answer[neighbor] + count[neighbor]
        }
    }

    // Pre-order DFS to calculate final answer
    var preOrder func(int, int)
    preOrder = func(node int, parent int) {
        for _, neighbor := range tree[node] {
            if neighbor == parent {
                continue
            }
            answer[neighbor] = answer[node] - count[neighbor] + (N - count[neighbor])
            preOrder(neighbor, node)
        }
    }

    postOrder(0, -1) // Start DFS from the root (node 0)
    preOrder(0, -1)  // Compute the final answers

    return answer
}
```

This approach uses Depth First Search (DFS) to first calculate the sum of distances from the root to all nodes and uses it to compute the distances for the rest by adjusting them based on the tree structure.

