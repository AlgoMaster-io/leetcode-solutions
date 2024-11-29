# 94. [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approach 1: Recursive Inorder Traversal

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
package main

import "fmt"

// TreeNode definition
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func inorderTraversal(root *TreeNode) []int {
    result := []int{}
    inorder(root, &result)
    return result
}

func inorder(node *TreeNode, result *[]int) {
    if node == nil {
        return // Base case: if the node is nil, return
    }

    inorder(node.Left, result)   // Recurse for the left subtree
    *result = append(*result, node.Val) // Visit the root
    inorder(node.Right, result)  // Recurse for the right subtree
}
```

## Approach 2: Iterative Inorder Traversal Using Stack

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
package main

// TreeNode definition
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func inorderTraversal(root *TreeNode) []int {
    result := []int{}
    stack := []*TreeNode{}
    current := root

    for current != nil || len(stack) > 0 {
        for current != nil {
            stack = append(stack, current) // Push the current node and move to the left subtree
            current = current.Left
        }

        current = stack[len(stack)-1] // Process the top node
        stack = stack[:len(stack)-1]
        result = append(result, current.Val) // Visit the node
        current = current.Right // Move to the right subtree
    }

    return result
}
```

## Approach 3: Morris Inorder Traversal (Without Recursion or Stack)

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
package main

// TreeNode definition
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func inorderTraversal(root *TreeNode) []int {
    result := []int{}
    current := root

    for current != nil {
        if current.Left == nil {
            result = append(result, current.Val) // Visit the node
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
                result = append(result, current.Val) // Visit the node
                current = current.Right // Move to the right subtree
            }
        }
    }

    return result
}
```

