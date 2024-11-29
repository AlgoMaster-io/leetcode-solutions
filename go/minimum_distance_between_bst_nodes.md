# 783. [Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

## Approach 1: Inorder Traversal with a List

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the inorder traversal
package main

import (
	"math"
)

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func minDiffInBST(root *TreeNode) int {
    var values []int
    inorder(root, &values)

    minDiff := math.MaxInt32
    for i := 1; i < len(values); i++ {
        minDiff = min(minDiff, values[i]-values[i-1])
    }
    return minDiff
}

func inorder(node *TreeNode, values *[]int) {
    if node == nil {
        return
    }

    inorder(node.Left, values) // Recurse for the left subtree
    *values = append(*values, node.Val) // Add the current node value
    inorder(node.Right, values) // Recurse for the right subtree
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

## Approach 2: Inorder Traversal with Constant Space

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
package main

import (
	"math"
)

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

var prev *int
var minDiff int

func minDiffInBST(root *TreeNode) int {
    prev = nil
    minDiff = math.MaxInt32
    inorder(root)
    return minDiff
}

func inorder(node *TreeNode) {
    if node == nil {
        return
    }

    inorder(node.Left) // Recurse for the left subtree

    if prev != nil {
        minDiff = min(minDiff, node.Val-*prev) // Calculate the difference with the previous value
    }
    val := node.Val
    prev = &val // Update the previous node value

    inorder(node.Right) // Recurse for the right subtree
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

## Approach 3: Morris Traversal (Space Optimized)

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
package main

import (
	"math"
)

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func minDiffInBST(root *TreeNode) int {
    var prev *TreeNode
    minDiff := math.MaxInt32
    current := root

    for current != nil {
        if current.Left == nil {
            // Process the current node
            if prev != nil {
                minDiff = min(minDiff, current.Val-prev.Val)
            }
            prev = current
            current = current.Right // Move to the right subtree
        } else {
            predecessor := current.Left

            // Find the rightmost node in the left subtree
            for predecessor.Right != nil && predecessor.Right != current {
                predecessor = predecessor.Right
            }

            if predecessor.Right == nil {
                predecessor.Right = current // Create a temporary thread to the root
                current = current.Left // Move to the left subtree
            } else {
                predecessor.Right = nil // Remove the temporary thread
                if prev != nil {
                    minDiff = min(minDiff, current.Val-prev.Val)
                }
                prev = current
                current = current.Right // Move to the right subtree
            }
        }
    }
    return minDiff
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

