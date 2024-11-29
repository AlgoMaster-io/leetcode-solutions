# 437. [Path Sum III](https://leetcode.com/problems/path-sum-iii/)

## Approach 1: Brute Force (DFS for Each Node)

### Solution
go
```go
// Time Complexity: O(n^2) in the worst case (skewed tree), O(n log n) for balanced tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func pathSum(root *TreeNode, targetSum int) int {
    if root == nil {
        return 0 // Base case: empty tree
    }

    // Calculate paths including the current root and recurse for left and right subtrees
    return dfs(root, targetSum) + pathSum(root.Left, targetSum) + pathSum(root.Right, targetSum)
}

func dfs(node *TreeNode, targetSum int) int {
    if node == nil {
        return 0 // Base case: empty node
    }

    count := 0
    if node.Val == targetSum {
        count++ // Path ends at the current node
    }

    // Check paths including left and right children
    count += dfs(node.Left, targetSum-node.Val)
    count += dfs(node.Right, targetSum-node.Val)

    return count
}
```

## Approach 2: Optimized Using Prefix Sum (HashMap)

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h) for recursion stack and O(n) for HashMap storage
package main

import "fmt"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func pathSum(root *TreeNode, targetSum int) int {
    prefixSumMap := make(map[int]int)
    prefixSumMap[0] = 1 // Base case: one way to get sum 0
    return dfs(root, 0, targetSum, prefixSumMap)
}

func dfs(node *TreeNode, currentSum, targetSum int, prefixSumMap map[int]int) int {
    if node == nil {
        return 0 // Base case: empty node
    }

    currentSum += node.Val
    paths := prefixSumMap[currentSum-targetSum] // Count paths ending at this node

    // Update the prefix sum map for this node
    prefixSumMap[currentSum]++

    // Recurse for left and right children
    paths += dfs(node.Left, currentSum, targetSum, prefixSumMap)
    paths += dfs(node.Right, currentSum, targetSum, prefixSumMap)

    // Backtrack: remove the current node's contribution to the prefix sum
    prefixSumMap[currentSum]--

    return paths
}
```

