# 543. [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

## Approach 1: Depth-First Search (DFS) with Recursive Depth Calculation

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
package main

// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

type Solution struct {
    diameter int
}

func (s *Solution) diameterOfBinaryTree(root *TreeNode) int {
    s.depth(root)
    return s.diameter
}

func (s *Solution) depth(node *TreeNode) int {
    if node == nil {
        return 0 // Base case: null node has depth 0
    }

    leftDepth := s.depth(node.Left)  // Calculate depth of left subtree
    rightDepth := s.depth(node.Right) // Calculate depth of right subtree

    // Update the diameter (maximum path length between two nodes)
    s.diameter = max(s.diameter, leftDepth+rightDepth)

    // Return the depth of the current subtree
    return max(leftDepth, rightDepth) + 1
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 2: Bottom-Up DFS with Global Variable (Optimized)

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
package main

// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

type Solution struct {
    maxDiameter int
}

func (s *Solution) diameterOfBinaryTree(root *TreeNode) int {
    s.calculateDepth(root)
    return s.maxDiameter
}

func (s *Solution) calculateDepth(node *TreeNode) int {
    if node == nil {
        return 0 // Null nodes contribute 0 to depth
    }

    left := s.calculateDepth(node.Left)  // Left subtree depth
    right := s.calculateDepth(node.Right) // Right subtree depth

    s.maxDiameter = max(s.maxDiameter, left+right) // Update the max diameter

    return max(left, right) + 1 // Return the current depth
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 3: Iterative DFS Using Stack

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
package main

import "container/list"

// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func diameterOfBinaryTree(root *TreeNode) int {
    if root == nil {
        return 0 // Edge case: empty tree
    }

    stack := list.New()
    depthMap := make(map[*TreeNode]int)
    maxDiameter := 0

    stack.PushBack(root)
    for stack.Len() > 0 {
        current := stack.Back().Value.(*TreeNode)
        if current == nil {
            stack.Remove(stack.Back())
            continue
        }

        if (current.Left != nil && depthMap[current.Left] == 0) || 
           (current.Right != nil && depthMap[current.Right] == 0) {
            if current.Left != nil {
                stack.PushBack(current.Left)
            }
            if current.Right != nil {
                stack.PushBack(current.Right)
            }
        } else {
            stack.Remove(stack.Back())
            left := depthMap[current.Left]
            right := depthMap[current.Right]
            maxDiameter = max(maxDiameter, left+right)
            depthMap[current] = max(left, right) + 1
        }
    }

    return maxDiameter
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

