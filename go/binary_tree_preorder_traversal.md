# 144. [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approach 1: Recursive Preorder Traversal

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
package main

import "fmt"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func preorderTraversal(root *TreeNode) []int {
    var result []int
    preorder(root, &result)
    return result
}

func preorder(node *TreeNode, result *[]int) {
    if node == nil {
        return // Base case: if the node is nil, return
    }

    *result = append(*result, node.Val) // Visit the root
    preorder(node.Left, result)         // Recurse for the left subtree
    preorder(node.Right, result)        // Recurse for the right subtree
}
```

## Approach 2: Iterative Preorder Traversal Using Stack

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
package main

import "fmt"

func preorderTraversal(root *TreeNode) []int {
    var result []int
    if root == nil {
        return result // Return empty result if the tree is empty
    }

    stack := []*TreeNode{root}

    for len(stack) > 0 {
        currentNode := stack[len(stack)-1]
        stack = stack[:len(stack)-1]

        result = append(result, currentNode.Val) // Visit the root

        // Push right child first so that left child is processed first
        if currentNode.Right != nil {
            stack = append(stack, currentNode.Right)
        }
        if currentNode.Left != nil {
            stack = append(stack, currentNode.Left)
        }
    }

    return result
}
```

## Approach 3: Iterative Preorder Traversal With Threaded Binary Tree

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used apart from output
package main

import "fmt"

func preorderTraversal(root *TreeNode) []int {
    var result []int
    current := root

    for current != nil {
        if current.Left == nil {
            result = append(result, current.Val) // Visit the root
            current = current.Right              // Move to the right
        } else {
            predecessor := current.Left

            // Find the rightmost node in the left subtree
            for predecessor.Right != nil && predecessor.Right != current {
                predecessor = predecessor.Right
            }

            if predecessor.Right == nil {
                result = append(result, current.Val) // Visit the root
                predecessor.Right = current          // Create a temporary thread to the root
                current = current.Left               // Move to the left
            } else {
                predecessor.Right = nil // Remove the temporary thread
                current = current.Right // Move to the right
            }
        }
    }

    return result
}
```

