# [Problem: Leetcode 101: Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

## Approaches

1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach (Using Queue)](#iterative-approach-using-queue)

## Recursive Approach

The basic idea to solve this problem is to use a helper function which checks if two trees are mirrors of each other. Here's the intuition:

- A tree is symmetric if the left subtree is a mirror reflection of the right subtree.
- Two trees are a mirror reflection of each other if:
  - Their roots have the same value.
  - The right subtree of each tree is a mirror reflection of the left subtree of the other tree.

### Go Code

```go
func isSymmetric(root *TreeNode) bool {
    if root == nil {
        return true
    }
    return isMirror(root.Left, root.Right)
}

func isMirror(left, right *TreeNode) bool {
    if left == nil && right == nil {
        return true
    }
    if left == nil || right == nil {
        return false
    }
    return (left.Val == right.Val) && isMirror(left.Right, right.Left) && isMirror(left.Left, right.Right)
}
```

### Complexity

- **Time Complexity:** O(n), where n is the number of nodes in the tree. Each node is visited once.
- **Space Complexity:** O(h), where h is the height of the tree. This accounts for the recursion stack used.

## Iterative Approach (Using Queue)

An iterative approach can also be used with the help of a queue (representing level-order traversal):

- Use a queue to traverse the tree iteratively.
- At each step, check the current pair of nodes.
- If they are not equal, the tree is not symmetric.
- Add their children to the queue in a specific order to mimic depth-first order: left-right of the first node with the right-left of the second node.

### Go Code

```go
func isSymmetric(root *TreeNode) bool {
    if root == nil {
        return true
    }
    
    queue := []*TreeNode{root.Left, root.Right}
    
    for len(queue) > 0 {
        left := queue[0]
        right := queue[1]
        queue = queue[2:]
        
        if left == nil && right == nil {
            continue
        }
        if left == nil || right == nil || left.Val != right.Val {
            return false
        }
        
        // Enqueue the children in the order: left subtree's left, right subtree's right
        // and left subtree's right, right subtree's left
        queue = append(queue, left.Left, right.Right, left.Right, right.Left)
    }
    
    return true
}
```

### Complexity

- **Time Complexity:** O(n), where n is the number of nodes in the tree. We traverse each node once.
- **Space Complexity:** O(n), for the space used by the queue which holds pairs of nodes. In the worst case, it could have all nodes of the last level in the tree.

