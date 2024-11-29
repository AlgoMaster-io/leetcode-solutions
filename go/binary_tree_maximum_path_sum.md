# 124. [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approach: Depth-First Search (DFS)

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

// TreeNode definition for the binary tree node
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func maxPathSum(root *TreeNode) int {
    maxSum := int(^uint(0) >> 1) * -1 // Initialize to the smallest possible integer
    calculateMaxPath(root, &maxSum)
    return maxSum
}

func calculateMaxPath(node *TreeNode, maxSum *int) int {
    if node == nil {
        return 0 // Base case: null node contributes 0 to the path sum
    }

    // Recursively calculate the maximum path sum of the left and right subtrees
    leftMax := max(0, calculateMaxPath(node.Left, maxSum))   // Ignore negative sums
    rightMax := max(0, calculateMaxPath(node.Right, maxSum)) // Ignore negative sums

    // Update the global maximum path sum
    *maxSum = max(*maxSum, leftMax+rightMax+node.Val)

    // Return the maximum path sum including the current node and one of its subtrees
    return max(leftMax, rightMax) + node.Val
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

