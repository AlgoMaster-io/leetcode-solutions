# [Leetcode 222: Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

In this problem, we are given a complete binary tree and asked to count the number of nodes present in the tree.

## Approaches
1. [Brute Force - Recursive Tree Traversal](#brute-force---recursive-tree-traversal)
2. [Optimized Recursive Approach Using Properties of Complete Tree](#optimized-recursive-approach-using-properties-of-complete-tree)
3. [Optimal Approach - Binary Search with Tree Height](#optimal-approach---binary-search-with-tree-height)

### Brute Force - Recursive Tree Traversal

In a complete binary tree, we can visit each node and count them using a simple recursive traversal. This approach involves traversing each node of the tree and incrementing a count.

**Intuition**:
- Use a recursive function to traverse all nodes.
- Base case is the null node where we return 0.
- Recurse through left and right subtrees and sum their counts including the current node.

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func countNodes(root *TreeNode) int {
    if root == nil {
        return 0
    }
    // Count the root node plus nodes from left and right subtree
    return 1 + countNodes(root.Left) + countNodes(root.Right)
}
```

**Time Complexity**: O(n) - We visit every node exactly once.  
**Space Complexity**: O(h) - The recursion stack space where h is the height of the tree.

### Optimized Recursive Approach Using Properties of Complete Tree

Since it's a complete binary tree, if the depth of the leftmost node is the same as the rightmost node, the tree is a perfect tree at this level. This means the total number of nodes is calculated directly using the formula for perfect trees.

**Intuition**:
- First, measure the left and right depths of the tree.
- If the depths are equal, the tree is full at that level, and nodes can be counted using `2^depth - 1`.
- If the depths differ, recurse into both left and right subtrees and sum their node counts.

```go
func countNodes(root *TreeNode) int {
    if root == nil {
        return 0
    }
    
    leftDepth := getDepth(root.Left)
    rightDepth := getDepth(root.Right)
    
    if leftDepth == rightDepth {
        // If left and right depths are same, left subtree is a full tree
        return (1 << leftDepth) + countNodes(root.Right)
    } else {
        // Else right subtree is a full tree of depth-1
        return (1 << rightDepth) + countNodes(root.Left)
    }
}

// Helper function to calculate depth
func getDepth(node *TreeNode) int {
    depth := 0
    for node != nil {
        depth++
        node = node.Left
    }
    return depth
}
```

**Time Complexity**: O(log(n) * log(n)) - Depth calculated at each recursive step.  
**Space Complexity**: O(log(n)) - Recursion stack proportional to the height of the tree.

### Optimal Approach - Binary Search with Tree Height

Taking advantage of the complete binary tree property, we can apply a binary search approach based on the height of the tree. This ensures the most efficient method to count nodes.

**Intuition**:
- Calculate the depth of the leftmost and rightmost path to determine height.
- Perform a binary search over the number of levels, checking existence with getNode path.
- Calculate total possible nodes in full levels, plus nodes counted via binary search.

```go
func countNodes(root *TreeNode) int {
    if root == nil {
        return 0
    }
    
    depth := getDepth(root)
    if depth == 0 {
        return 1
    }
    
    // Last level nodes lie between 1 and 2^depth where depth is the last level depth
    left, right := 1, (1 << depth) - 1
    while left <= right {
        mid := left + (right-left)/2
        if nodeExists(mid, depth, root) {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    
    // Total nodes = nodes from full levels + nodes from last level
    return (1 << depth) - 1 + left
}

// Helper function to calculate depth
func getDepth(node *TreeNode) int {
    depth := 0
    for node != nil {
        depth++
        node = node.Left
    }
    return depth
}

// Helper function to determine if node at given index exists
func nodeExists(idx int, depth int, node *TreeNode) bool {
    left, right := 0, (1 << depth) - 1
    for i := 0; i < depth; i++ {
        mid := left + (right-left)/2
        if idx <= mid {
            node = node.Left
            right = mid
        } else {
            node = node.Right
            left = mid + 1
        }
    }
    return node != nil
}
```

**Time Complexity**: O(log(n)^2) - Binary search times checking node existence.  
**Space Complexity**: O(1) - No recursion stack used, constant space.

