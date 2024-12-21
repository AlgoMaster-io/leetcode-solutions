# [Binary Tree Maximum Path Sum - LeetCode](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Table of Contents
1. [Approach 1: Brute Force](#approach-1)
2. [Approach 2: Optimal Solution with Recursion](#approach-2)

---

## Approach 1: Brute Force

### Intuition
The brute force solution involves checking every possible path in the binary tree, calculating the sum for each path, and keeping track of the maximum sum encountered. A path can be defined as any sequence of nodes from some starting node to any descendant node along the parent-child connections.

### Approach
1. Traverse the tree using DFS.
2. For each node, consider the path that includes:
   - The node itself.
   - The node plus the maximum path in the left subtree.
   - The node plus the maximum path in the right subtree.
3. Calculate the overall max path sum encountered so far, ensuring to include paths that use both left and right children.

### Code

```go
package main

import (
    "math"
)

// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func maxPathSum(root *TreeNode) int {
    var maxSum int = math.MinInt32
    
    // Helper function to calculate max path sum
    var findMaxPath func(node *TreeNode) int
    findMaxPath = func(node *TreeNode) int {
        if node == nil {
            return 0
        }
        
        // Calculate left and right path sums
        leftMax := findMaxPath(node.Left)
        rightMax := findMaxPath(node.Right)
        
        // Maximum single path sum using this node
        maxSingle := max(node.Val, node.Val + max(leftMax, rightMax))
        
        // Maximum combined path sum with both children
        maxTop := max(maxSingle, node.Val + leftMax + rightMax)
        
        // Update the overall maximum path sum
        maxSum = max(maxSum, maxTop)
        
        return maxSingle
    }
    
    findMaxPath(root)
    return maxSum
}

// Utility function to get maximum of two integers
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity
- **Time Complexity**: O(N), where N is the number of nodes in the binary tree. We visit each node once.
- **Space Complexity**: O(H), where H is the height of the binary tree due to recursion stack space.

---

## Approach 2: Optimal Solution with Recursion

### Intuition
The optimal solution leverages a postorder traversal (left-right-node) to compute the maximum path sum. We calculate the maximum gain from both subtrees while ensuring we propagate only the greater path contributions up the recursive stack combined with the current node value.

### Approach
1. Define a recursive function which calculates the maximum gain from any given node.
2. Base case: if the node is `nil`, return 0.
3. Recursively calculate the maximum gains from the left and right subtrees.
4. Update global maximum path sum considering the possibility that the current node could serve as a bridge in the path.
5. Return the maximum gain obtained by including the current node with either its left or right subtree to be used by its parent node computation.

### Code

```go
package main

import (
    "math"
)

// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func maxPathSum(root *TreeNode) int {
    var maxSum int = math.MinInt32
    
    var maxGain func(node *TreeNode) int
    maxGain = func(node *TreeNode) int {
        if node == nil {
            return 0
        }
        
        // Calculate gains from left and right subtrees
        leftGain := max(maxGain(node.Left), 0)  // Only consider positive contributions
        rightGain := max(maxGain(node.Right), 0) // Only consider positive contributions
        
        // Calculate the price_newpath
        priceNewPath := node.Val + leftGain + rightGain
        
        // Update max_sum if priceNewPath is better
        maxSum = max(maxSum, priceNewPath)
        
        // For recursion, return the max gain when continuing to either left or right
        return node.Val + max(leftGain, rightGain)
    }
    
    maxGain(root)
    return maxSum
}

// Utility function to get max of two integers
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity
- **Time Complexity**: O(N) due to visiting each node in the tree.
- **Space Complexity**: O(H), where H is the height of the tree, used by the recursion stack.

This optimized approach achieves linear time complexity by computing the result in one traversal, making it efficient and scalable even for larger trees.

