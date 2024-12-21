# [Leetcode 144: Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approaches

- [Recursive Approach](#recursive-approach)
- [Iterative Approach with Stack](#iterative-approach-with-stack)
- [Morris Traversal](#morris-traversal)

### Recursive Approach

**Intuition:**

A binary tree can be traversed recursively in pre-order by visiting the root node first, then recursively traversing the left subtree, and finally the right subtree. This recursive approach is straightforward and matches the natural definition of pre-order traversal.

**Implementation:**

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func preorderTraversal(root *TreeNode) []int {
    var result []int

    // Helper function to perform recursive traversal.
    var traverse func(node *TreeNode)
    traverse = func(node *TreeNode) {
        if node == nil {
            return
        }
        // Visit the root node.
        result = append(result, node.Val)

        // Traverse the left subtree.
        traverse(node.Left)

        // Traverse the right subtree.
        traverse(node.Right)
    }

    // Start the traversal with the root node.
    traverse(root)

    return result
}
```

**Time Complexity:** O(n)  
- We visit each node exactly once.

**Space Complexity:** O(h)  
- The call stack will hold at most h recursive calls, where h is the height of the tree. In the worst case, this is O(n) for a skewed tree.

### Iterative Approach with Stack

**Intuition:**

Instead of using recursion, we can use an explicit stack to store nodes. We start from the root node, push it onto the stack, and then process each node by pushing its right and then left child onto the stack. By always pushing the right child before the left child, we ensure that the left child is processed sooner, which aligns with the pre-order traversal order.

**Implementation:**

```go
func preorderTraversal(root *TreeNode) []int {
    if root == nil {
        return []int{}
    }

    var result []int
    stack := []*TreeNode{root}

    // While there are nodes to process in the stack.
    for len(stack) > 0 {
        // Pop the last node from the stack.
        node := stack[len(stack)-1]
        stack = stack[:len(stack)-1]

        // Visit the node.
        result = append(result, node.Val)

        // Push right child first so that left child is processed first.
        if node.Right != nil {
            stack = append(stack, node.Right)
        }

        // Push left child.
        if node.Left != nil {
            stack = append(stack, node.Left)
        }
    }

    return result
}
```

**Time Complexity:** O(n)  
- We visit each node exactly once.

**Space Complexity:** O(n)  
- The stack may contain up to n nodes, especially in a fully unbalanced binary tree.

### Morris Traversal

**Intuition:**

Morris traversal is a method to traverse a binary tree without using recursion or stack by making use of tree manipulation. In pre-order traversal, the idea is to temporarily change the tree structure to make it possible to return to any node after visiting its left subtree.

1. Start at the root node.
2. While the current node is not null, check if the node has a left child.
3. If it has, find the rightmost node in its left subtree (predecessor).
4. If the predecessor's right child is null, make the current node the right child of the predecessor and move to the left child of the current node.
5. If the predecessor's right child is the current node, restore the tree by setting the predecessor's right child to null, and move to the right child of the current node.

**Implementation:**

```go
func preorderTraversal(root *TreeNode) []int {
    var result []int
    current := root

    // While there are nodes to process.
    for current != nil {
        if current.Left == nil {
            // Visit the current node if no left subtree.
            result = append(result, current.Val)
            current = current.Right
        } else {
            // Find the predecessor.
            predecessor := current.Left
            for predecessor.Right != nil && predecessor.Right != current {
                predecessor = predecessor.Right
            }

            // If predecessor's right is null, visit the node and make link to the current node.
            if predecessor.Right == nil {
                result = append(result, current.Val) // Visit the node.
                predecessor.Right = current          // Make a temporary link.
                current = current.Left               // Move to the left child.
            } else {
                // Restore the tree by removing the temporary link.
                predecessor.Right = nil
                current = current.Right              // Move to the right child.
            }
        }
    }

    return result
}
```

**Time Complexity:** O(n)  
- Each edge is visited at most twice, once to create a link and once to restore the tree.

**Space Complexity:** O(1)  
- Utilizes no stack or extra recursive call space; the tree's structure is temporarily modified.

