# 102. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## Approach 1: Breadth-First Search (Using Queue)

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage
package main

import "fmt"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func levelOrder(root *TreeNode) [][]int {
    var result [][]int
    if root == nil {
        return result // Return empty result if the tree is empty
    }

    queue := []*TreeNode{root}

    for len(queue) > 0 {
        levelSize := len(queue)
        var currentLevel []int

        for i := 0; i < levelSize; i++ {
            currentNode := queue[0]
            queue = queue[1:] // Dequeue the first element
            currentLevel = append(currentLevel, currentNode.Val)

            if currentNode.Left != nil {
                queue = append(queue, currentNode.Left) // Add left child to the queue
            }
            if currentNode.Right != nil {
                queue = append(queue, currentNode.Right) // Add right child to the queue
            }
        }

        result = append(result, currentLevel) // Add the current level to the result
    }

    return result
}

func main() {
    // Example usage:
    root := &TreeNode{Val: 3}
    root.Left = &TreeNode{Val: 9}
    root.Right = &TreeNode{Val: 20, Left: &TreeNode{Val: 15}, Right: &TreeNode{Val: 7}}

    fmt.Println(levelOrder(root)) // Output: [[3] [9 20] [15 7]]
}
```

## Approach 2: Depth-First Search (Recursive)

### Solution
go
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

func levelOrderDFS(root *TreeNode) [][]int {
    var result [][]int
    dfs(root, 0, &result)
    return result
}

func dfs(node *TreeNode, level int, result *[][]int) {
    if node == nil {
        return // Base case: if the node is null, return
    }
    
    if level == len(*result) {
        *result = append(*result, []int{}) // Create a new level in the result
    }
    
    (*result)[level] = append((*result)[level], node.Val) // Add current node's value to the current level

    dfs(node.Left, level+1, result)  // Recurse for the left child
    dfs(node.Right, level+1, result) // Recurse for the right child
}

func main() {
    // Example usage:
    root := &TreeNode{Val: 3}
    root.Left = &TreeNode{Val: 9}
    root.Right = &TreeNode{Val: 20, Left: &TreeNode{Val: 15}, Right: &TreeNode{Val: 7}}

    fmt.Println(levelOrderDFS(root)) // Output: [[3] [9 20] [15 7]]
}
```

