# [Leetcode 94: Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approaches
- [Approach 1: Recursive Inorder Traversal](#approach-1-recursive-inorder-traversal)
- [Approach 2: Iterative Inorder Traversal using Stack](#approach-2-iterative-inorder-traversal-using-stack)
- [Approach 3: Morris Traversal](#approach-3-morris-traversal)

## Approach 1: Recursive Inorder Traversal

### Intuition
The recursive approach to binary tree traversal leverages the natural recursive structure of trees. For inorder traversal, the process can be broken down to:
1. Traverse the left subtree.
2. Visit the node.
3. Traverse the right subtree.

This approach uses the call stack to maintain the order of operations, ensuring that any node's left subtree is fully processed before visiting the node itself.

### Go Code
```go
// TreeNode represents a node in a binary tree.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func inorderTraversal(root *TreeNode) []int {
    // This slice will hold the inorder traversal result.
    result := []int{}
    // Helper function to perform recursion.
    var helper func(node *TreeNode)
    helper = func(node *TreeNode) {
        if node != nil {
            helper(node.Left)           // Traverse left subtree
            result = append(result, node.Val) // Visit node
            helper(node.Right)          // Traverse right subtree
        }
    }
    helper(root)
    return result
}
```

### Time and Space Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is visited once.
- **Space Complexity**: O(h), where h is the height of the tree, representing the depth of the recursion stack (worst case O(n) for skewed trees).

## Approach 2: Iterative Inorder Traversal using Stack

### Intuition
The iterative approach uses an explicit stack to mimic the call stack mechanism used in recursion. The idea is to push nodes onto the stack until we reach the leftmost node, then process nodes stored in the stack, ensuring the left nodes are processed before the current node, and then process the right subtree.

### Go Code
```go
func inorderTraversalIterative(root *TreeNode) []int {
    result := []int{}
    stack := []*TreeNode{}
    current := root

    for current != nil || len(stack) > 0 {
        // Move to the leftmost node
        for current != nil {
            stack = append(stack, current)
            current = current.Left
        }
        // Pop from the stack
        last := stack[len(stack)-1]
        stack = stack[:len(stack)-1]

        // Append the node value to result
        result = append(result, last.Val)

        // Now, we'll visit the right subtree
        current = last.Right
    }

    return result
}
```

### Time and Space Complexity
- **Time Complexity**: O(n).
- **Space Complexity**: O(h) for the stack, where h is the height of the tree.

## Approach 3: Morris Traversal

### Intuition
Morris traversal aims to perform the inorder traversal with O(1) space by modifying the tree during the traversal process:
1. If the left child is null, process the current node and move to the right child.
2. If the left child is not null, find the inorder predecessor (the rightmost node in the left subtree).
3. If the right child of the predecessor is null, make the current node the right child of the predecessor. Move to the left child.
4. If the right child of the predecessor is the current node, revert the changes (make it null), process the current node, and move to the right child.

### Go Code
```go
func inorderTraversalMorris(root *TreeNode) []int {
    result := []int{}
    current := root
    
    for current != nil {
        if current.Left == nil {
            // If there is no left child, record the value and move right
            result = append(result, current.Val)
            current = current.Right
        } else {
            // Find the inorder predecessor of current
            predecessor := current.Left
            for predecessor.Right != nil && predecessor.Right != current {
                predecessor = predecessor.Right
            }
            
            if predecessor.Right == nil {
                // Make current the right child of its inorder predecessor
                predecessor.Right = current
                current = current.Left
            } else {
                // Revert the changes made to restore the original tree structure
                predecessor.Right = nil
                result = append(result, current.Val)
                current = current.Right
            }
        }
    }

    return result
}
```

### Time and Space Complexity
- **Time Complexity**: O(n).
- **Space Complexity**: O(1) since no additional space is used except for a few pointers.

