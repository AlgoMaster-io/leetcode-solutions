# 199. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

## Approach 1: Breadth-First Search (Using Queue)

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage
import "container/list"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func rightSideView(root *TreeNode) []int {
    result := []int{}
    if root == nil {
        return result // Return empty result if the tree is empty
    }

    queue := list.New()
    queue.PushBack(root)

    for queue.Len() > 0 {
        levelSize := queue.Len()
        var lastNode *TreeNode

        for i := 0; i < levelSize; i++ {
            currentNode := queue.Remove(queue.Front()).(*TreeNode)
            lastNode = currentNode

            if currentNode.Left != nil {
                queue.PushBack(currentNode.Left) // Add left child to the queue
            }
            if currentNode.Right != nil {
                queue.PushBack(currentNode.Right) // Add right child to the queue
            }
        }

        result = append(result, lastNode.Val) // Add the last node's value from this level
    }

    return result
}
```

## Approach 2: Depth-First Search (Recursive)

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func rightSideView(root *TreeNode) []int {
    result := []int{}
    dfs(root, 0, &result)
    return result
}

func dfs(node *TreeNode, depth int, result *[]int) {
    if node == nil {
        return // Base case: if the node is nil, return
    }

    if depth == len(*result) {
        *result = append(*result, node.Val) // Add the first node of this depth (rightmost node)
    }

    dfs(node.Right, depth+1, result) // Visit the right subtree first
    dfs(node.Left, depth+1, result)  // Visit the left subtree next
}
```

