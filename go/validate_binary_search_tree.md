# 98. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Approach 1: Inorder Traversal (Iterative)

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
package main

import "fmt"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func isValidBST(root *TreeNode) bool {
    stack := []*TreeNode{}
    current := root
    var prev *TreeNode

    for current != nil || len(stack) > 0 {
        for current != nil {
            stack = append(stack, current) // Push nodes of the left subtree
            current = current.Left
        }

        current = stack[len(stack)-1]
        stack = stack[:len(stack)-1] // Process the top node

        if prev != nil && current.Val <= prev.Val {
            return false // If the current value is not greater than the previous, it's invalid
        }
        prev = current

        current = current.Right // Move to the right subtree
    }

    return true
}

func main() {
    node := &TreeNode{2, &TreeNode{1, nil, nil}, &TreeNode{3, nil, nil}}
    fmt.Println(isValidBST(node)) // Should print true
}
```

## Approach 2: Recursive Inorder Traversal

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
package main

import "fmt"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

var prev *TreeNode

func isValidBST(root *TreeNode) bool {
    prev = nil
    return inorder(root)
}

func inorder(node *TreeNode) bool {
    if node == nil {
        return true // Base case: empty tree is valid
    }

    if !inorder(node.Left) {
        return false // Left subtree is invalid
    }

    if prev != nil && node.Val <= prev.Val {
        return false // Current value is not greater than the previous
    }
    prev = node // Update the previous node

    return inorder(node.Right) // Recurse for the right subtree
}

func main() {
    node := &TreeNode{2, &TreeNode{1, nil, nil}, &TreeNode{3, nil, nil}}
    fmt.Println(isValidBST(node)) // Should print true
}
```

## Approach 3: DFS with Range Validation

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
package main

import "fmt"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func isValidBST(root *TreeNode) bool {
    return validate(root, nil, nil)
}

func validate(node *TreeNode, lower *int, upper *int) bool {
    if node == nil {
        return true // Base case: empty tree is valid
    }

    if (lower != nil && node.Val <= *lower) || (upper != nil && node.Val >= *upper) {
        return false // Node value violates the range constraints
    }

    // Recursively validate the left and right subtrees
    return validate(node.Left, lower, &node.Val) && validate(node.Right, &node.Val, upper)
}

func main() {
    node := &TreeNode{2, &TreeNode{1, nil, nil}, &TreeNode{3, nil, nil}}
    fmt.Println(isValidBST(node)) // Should print true
}
```

