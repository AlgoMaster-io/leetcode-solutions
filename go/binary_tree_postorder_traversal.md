# 145. [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approach 1: Recursive Postorder Traversal

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func postorderTraversal(root *TreeNode) []int {
    var result []int
    postorder(root, &result)
    return result
}

func postorder(node *TreeNode, result *[]int) {
    if node == nil {
        return // Base case: if the node is nil, return
    }

    postorder(node.Left, result)  // Recurse for the left subtree
    postorder(node.Right, result) // Recurse for the right subtree
    *result = append(*result, node.Val) // Visit the root
}
```

## Approach 2: Iterative Postorder Traversal Using Two Stacks

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(2n) = O(n), for two stacks
package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func postorderTraversal(root *TreeNode) []int {
    var result []int
    if root == nil {
        return result
    }

    stack1 := []*TreeNode{}
    stack2 := []*TreeNode{}
    stack1 = append(stack1, root)

    for len(stack1) > 0 {
        current := stack1[len(stack1)-1]
        stack1 = stack1[:len(stack1)-1]
        stack2 = append(stack2, current)

        if current.Left != nil {
            stack1 = append(stack1, current.Left) // Push left child to stack1
        }
        if current.Right != nil {
            stack1 = append(stack1, current.Right) // Push right child to stack1
        }
    }

    for len(stack2) > 0 {
        node := stack2[len(stack2)-1]
        stack2 = stack2[:len(stack2)-1]
        result = append(result, node.Val) // Add nodes in postorder from stack2
    }

    return result
}
```

## Approach 3: Iterative Postorder Traversal Using One Stack

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func postorderTraversal(root *TreeNode) []int {
    var result []int
    if root == nil {
        return result
    }

    stack := []*TreeNode{}
    var lastVisited *TreeNode
    current := root

    for current != nil || len(stack) > 0 {
        if current != nil {
            stack = append(stack, current) // Push nodes to the stack
            current = current.Left // Move to the left subtree
        } else {
            peekNode := stack[len(stack)-1]
            if peekNode.Right != nil && lastVisited != peekNode.Right {
                current = peekNode.Right // Move to the right subtree
            } else {
                result = append(result, peekNode.Val) // Visit the node
                lastVisited = stack[len(stack)-1]
                stack = stack[:len(stack)-1]
            }
        }
    }

    return result
}
```

## Approach 4: Morris Postorder Traversal (Space Optimized)

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func postorderTraversal(root *TreeNode) []int {
    var result []int
    dummy := &TreeNode{Left: root}
    current := dummy

    for current != nil {
        if current.Left == nil {
            current = current.Right
        } else {
            predecessor := current.Left
            for predecessor.Right != nil && predecessor.Right != current {
                predecessor = predecessor.Right
            }

            if predecessor.Right == nil {
                predecessor.Right = current // Create a temporary thread
                current = current.Left
            } else {
                reverseAddPath(current.Left, predecessor, &result)
                predecessor.Right = nil // Remove the temporary thread
                current = current.Right
            }
        }
    }

    return result
}

func reverseAddPath(start, end *TreeNode, result *[]int) {
    var temp []int
    for node := start; node != end; node = node.Right {
        temp = append(temp, node.Val)
    }
    temp = append(temp, end.Val)
    for i := len(temp) - 1; i >= 0; i-- {
        *result = append(*result, temp[i]) // Add nodes in reverse order
    }
}
```

