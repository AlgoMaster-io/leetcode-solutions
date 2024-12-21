# [LeetCode 114: Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

## Approaches

1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach using Stack](#iterative-approach-using-stack)
3. [Optimal Morris Traversal Approach](#optimal-morris-traversal-approach)

---

## Recursive Approach

The basic intuition for this recursive approach is to perform a postorder traversal, where we recursively flatten the left and right subtrees and then stitch them together by manipulating pointers.

### Intuition

1. If the current node is `null`, return.
2. Recursively flatten the left and right subtrees.
3. Save the right subtree.
4. Move the left subtree and connect it to the right.
5. Append the previously saved right subtree to the new right by traversing to the end of the current right subtree.

### Code

```go
func flatten(root *TreeNode) {
    if root == nil {
        return
    }
    
    // Recursively flatten the left and right children
    flatten(root.Left)
    flatten(root.Right)
    
    // Store the right subtree and move the left subtree to the right
    rightSubtree := root.Right
    root.Right = root.Left
    root.Left = nil // Don't forget to set the left to nil
    
    // Find the end of the current right and connect the stored right subtree
    current := root
    for current.Right != nil {
        current = current.Right
    }
    current.Right = rightSubtree
}
```

### Complexity

- **Time Complexity**: O(n), where n is the number of nodes. Each node is visited once.
- **Space Complexity**: O(n) due to the recursion stack.

---

## Iterative Approach using Stack

This approach uses a stack to iteratively traverse the tree and flatten it. 

### Intuition

1. Use a stack to perform a depth-first traversal of the tree.
2. Pop a node from the stack, and push its right and left children onto the stack.
3. If the stack is not empty, set the current node’s right to the top of the stack.
4. Set the left of the current node to `null`.

### Code

```go
func flatten(root *TreeNode) {
    if root == nil {
        return
    }
    
    stack := []*TreeNode{}
    stack = append(stack, root)
    
    for len(stack) > 0 {
        node := stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        
        if node.Right != nil {
            stack = append(stack, node.Right)
        }
        if node.Left != nil {
            stack = append(stack, node.Left)
        }
        
        // If the stack still has elements, set the current node's right to the top of the stack
        if len(stack) > 0 {
            node.Right = stack[len(stack)-1]
        }
        
        // Set the left of the current node to nil
        node.Left = nil
    }
}
```

### Complexity

- **Time Complexity**: O(n), where n is the number of nodes.
- **Space Complexity**: O(n) due to the usage of the stack.

---

## Optimal Morris Traversal Approach

Morris Traversal allows us to perform operations in constant space by using the tree's structure itself.

### Intuition

1. For each node, find its predecessor (rightmost node in its left subtree).
2. If the predecessor exists, connect it to the current node’s right.
3. Move the left subtree under the right and set the left to `null`.
4. Continue to the next node to the right (which might be a predecessor).

### Code

```go
func flatten(root *TreeNode) {
    if root == nil {
        return
    }
    
    curr := root
    for curr != nil {
        if curr.Left != nil {
            // Find the rightmost node in the left subtree
            predecessor := curr.Left
            for predecessor.Right != nil {
                predecessor = predecessor.Right
            }
            // Connect it to the current node's right subtree
            predecessor.Right = curr.Right
            
            // Move left subtree to the right and disconnect left subtree
            curr.Right = curr.Left
            curr.Left = nil
        }
        // Move to the next node
        curr = curr.Right
    }
}
```

### Complexity

- **Time Complexity**: O(n)
- **Space Complexity**: O(1), as we are reusing the tree's structure.

---

Each approach is explained with the aim to gradually improve on efficiency, going from a recursive method that is simple but uses extra space, to a stack-based iterative approach, finally reaching an optimal Morris Traversal approach that requires minimal space.

