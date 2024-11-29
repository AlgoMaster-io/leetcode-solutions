# 103. [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Approach 1: Breadth-First Search with Direction Toggle

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage
package main

import "container/list"

type TreeNode struct {
    Val int
    Left, Right *TreeNode
}

func zigzagLevelOrder(root *TreeNode) [][]int {
    var result [][]int
    if root == nil {
        return result // Return empty result if the tree is empty
    }

    queue := list.New()
    queue.PushBack(root)
    leftToRight := true

    for queue.Len() > 0 {
        levelSize := queue.Len()
        currentLevel := list.New()

        for i := 0; i < levelSize; i++ {
            currentNode := queue.Remove(queue.Front()).(*TreeNode) // Process the current node

            if leftToRight {
                currentLevel.PushBack(currentNode.Val) // Add at the end
            } else {
                currentLevel.PushFront(currentNode.Val) // Add at the beginning
            }

            if currentNode.Left != nil {
                queue.PushBack(currentNode.Left) // Add left child to the queue
            }
            if currentNode.Right != nil {
                queue.PushBack(currentNode.Right) // Add right child to the queue
            }
        }

        level := make([]int, currentLevel.Len())
        index := 0
        for e := currentLevel.Front(); e != nil; e = e.Next() {
            level[index] = e.Value.(int)
            index++
        }
        result = append(result, level) // Add the current level to the result
        leftToRight = !leftToRight // Toggle the direction for the next level
    }

    return result
}
```

## Approach 2: Depth-First Search (Recursive with Level Tracking)

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

type TreeNode struct {
    Val int
    Left, Right *TreeNode
}

func zigzagLevelOrder(root *TreeNode) [][]int {
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

    if level % 2 == 0 {
        // Add normally for even levels
        (*result)[level] = append((*result)[level], node.Val)
    } else {
        // Add to the front for odd levels
        (*result)[level] = append([]int{node.Val}, (*result)[level]...)
    }

    dfs(node.Left, level + 1, result) // Recurse for the left child
    dfs(node.Right, level + 1, result) // Recurse for the right child
}
```

