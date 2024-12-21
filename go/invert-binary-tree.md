# [Leetcode Problem 226: Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach Using Queue (BFS)](#iterative-approach-using-queue-bfs)
3. [Iterative Approach Using Stack (DFS)](#iterative-approach-using-stack-dfs)

### Recursive Approach

#### Intuition:
The recursive approach takes advantage of tree properties and performs a depth-first traversal. The main idea is to swap the left and right children of each node. This can be easily achieved by recursively calling the invert function on each of the left and right subtrees.

#### Steps:
1. If the current node is `nil`, return `nil` (base case).
2. Recursively call the invert function on the left subtree.
3. Recursively call the invert function on the right subtree.
4. Swap the left and right children of the current node.
5. Return the current node after inverting the subtrees.

#### Code:
```go
// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func invertTree(root *TreeNode) *TreeNode {
    // Base case: if the node is nil, return nil
    if root == nil {
        return nil
    }

    // Recursively invert the left subtree
    left := invertTree(root.Left)
    // Recursively invert the right subtree
    right := invertTree(root.Right)

    // Swap the left and right subtrees
    root.Left = right
    root.Right = left

    // Return the current node
    return root
}
```

#### Complexity:
- Time Complexity: O(n), where n is the number of nodes in the tree.
- Space Complexity: O(h), where h is the height of the tree, due to the recursion stack.

### Iterative Approach Using Queue (BFS)

#### Intuition:
This approach uses a queue to perform a breadth-first traversal of the tree. At each step, it swaps the children of the current node and continues the process level by level.

#### Steps:
1. If the root is `nil`, return `nil`.
2. Initialize a queue and enqueue the root node.
3. While the queue is not empty:
   - Dequeue a node.
   - Swap its left and right children.
   - Enqueue the (possibly new) left child if it exists.
   - Enqueue the (possibly new) right child if it exists.
4. Return the root.

#### Code:
```go
func invertTree(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }

    // Initialize a queue to hold nodes for BFS
    queue := []*TreeNode{root}

    // Iterate until the queue is empty
    for len(queue) > 0 {
        // Dequeue the next node
        node := queue[0]
        queue = queue[1:]

        // Swap the left and right children
        node.Left, node.Right = node.Right, node.Left

        // Enqueue the children to process them later
        if node.Left != nil {
            queue = append(queue, node.Left)
        }
        if node.Right != nil {
            queue = append(queue, node.Right)
        }
    }

    // Return the inverted tree's root
    return root
}
```

#### Complexity:
- Time Complexity: O(n), where n is the number of nodes in the tree.
- Space Complexity: O(n), where n could be the width of the tree at the maximum.

### Iterative Approach Using Stack (DFS)

#### Intuition:
This approach employs a stack to perform a depth-first traversal of the tree. It is similar to the recursive approach but uses an explicit stack data structure to manage the nodes.

#### Steps:
1. If the root is `nil`, return `nil`.
2. Initialize a stack and push the root node onto it.
3. While the stack is not empty:
   - Pop a node from the stack.
   - Swap its left and right children.
   - Push the (possibly new) left child onto the stack if it exists.
   - Push the (possibly new) right child onto the stack if it exists.
4. Return the root.

#### Code:
```go
func invertTree(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }

    // Initialize a stack to hold nodes for DFS
    stack := []*TreeNode{root}

    // Iterate until the stack is empty
    for len(stack) > 0 {
        // Pop the top node from the stack
        node := stack[len(stack)-1]
        stack = stack[:len(stack)-1]

        // Swap the left and right children
        node.Left, node.Right = node.Right, node.Left

        // Push the children onto the stack for further processing
        if node.Left != nil {
            stack = append(stack, node.Left)
        }
        if node.Right != nil {
            stack = append(stack, node.Right)
        }
    }

    // Return the inverted tree's root
    return root
}
```

#### Complexity:
- Time Complexity: O(n), where n is the number of nodes in the tree.
- Space Complexity: O(h), where h is the height of the tree, similar to recursion stack in DFS.

