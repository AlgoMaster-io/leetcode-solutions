# [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

## Approaches

1. [Depth-First Search (DFS) Recursive Approach](#approach-1)
2. [Breadth-First Search (BFS) Iterative Approach](#approach-2)

### Approach 1: Depth-First Search (DFS) Recursive Approach

**Intuition:**

The problem requires us to obtain the rightmost view of the binary tree. A depth-first search starting from the root and prioritizing right children before left will naturally move through the tree in such a way that when we reach new levels, the first node we're recording is the rightmost node for that level. 

The approach involves recursively visiting nodes starting from the root. For each node, check if you are visiting that depth for the first time. If so, record the node's value as part of the right view since it’s the first node encountered at that depth.

**Algorithm:**

1. Initiate a recursive function with the current node and depth level.
2. If the current node is `nil`, return.
3. If the current depth is equal to the size of the result list, append the current node's value to the result.
4. Recursively visit the right child first, then the left child (right-to-left preorder).
5. Return the result list after the DFS completes.

**Code:**

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func rightSideView(root *TreeNode) []int {
    result := []int{}
    dfs(root, 0, &result)
    return result
}

func dfs(node *TreeNode, depth int, result *[]int) {
    if node == nil {
        return
    }
    
    // If this is the first time visiting this depth level, add the node's value.
    if depth == len(*result) {
        *result = append(*result, node.Val)
    }
    
    // Visit the right node first to ensure rightmost nodes are considered
    dfs(node.Right, depth+1, result)
    // Visit the left node after to consider any potential left view at deeper levels
    dfs(node.Left, depth+1, result)
}
```

**Time Complexity:** O(N), where N is the number of nodes in the tree. Each node is visited once.

**Space Complexity:** O(H), where H is the height of the tree. This space is used by the function call stack in a worst-case scenario (O(N) in a skewed tree).

### Approach 2: Breadth-First Search (BFS) Iterative Approach

**Intuition:**

A level-order traversal (BFS) can be performed to naturally handle nodes level by level. By traversing each level completely before moving onto the next, we can easily collect the last node of each level to form the right side view.

Using a queue, we process each node level by level. For each level, enqueue all children of each node. The last node processed in a particular level will represent the rightmost node of that level.

**Algorithm:**

1. Initialize a queue starting with the root node.
2. While the queue is not empty, perform the following:
   - Determine the current level size by the length of the queue.
   - For each node in the level, process it by popping from the queue, and then append its children to the queue.
   - Record the value of the last node processed in the current level (rightmost node for that level).
3. Continue this until all levels are processed.
4. Return the result list containing the right side view.

**Code:**

```go
func rightSideView(root *TreeNode) []int {
    if root == nil {
        return []int{}
    }

    result := []int{}
    queue := []*TreeNode{root}
    
    for len(queue) > 0 {
        levelSize := len(queue)
        var currentNode *TreeNode
        
        for i := 0; i < levelSize; i++ {
            // Dequeue the next node from the front
            currentNode = queue[0]
            queue = queue[1:]
            
            // Enqueue the left and right children if they exist
            if currentNode.Left != nil {
                queue = append(queue, currentNode.Left)
            }
            if currentNode.Right != nil {
                queue = append(queue, currentNode.Right)
            }
        }
        
        // The last processed node in this level is the rightmost node
        result = append(result, currentNode.Val)
    }
    
    return result
}
```

**Time Complexity:** O(N), where N is the number of nodes in the tree. Every node is processed once.

**Space Complexity:** O(D), where D is the maximum width of the tree, as it determines the maximum number of nodes in the queue at any time (at the largest level of the tree).

