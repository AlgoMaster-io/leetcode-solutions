# [Leetcode 543: Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

## Approaches:
- [Approach 1: Recursive Depth-First Search](#approach-1-recursive-depth-first-search)
- [Approach 2: Optimized Recursive DFS with Global Variable](#approach-2-optimized-recursive-dfs-with-global-variable)

## Approach 1: Recursive Depth-First Search

### Intuition:
The problem asks for the longest path between any two nodes in a tree. This can be achieved by considering it as the sum of the heights (depths) of the left and right subtrees of each node. Therefore, by recursively computing the depth of left and right subtrees for each node, we can determine the diameter by evaluating this depth sum.

### Code:
```go
// TreeNode represents a single node in a binary tree.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func diameterOfBinaryTree(root *TreeNode) int {
    // Helper function to compute the depth of a tree and update the diameter.
    var maxDiameter int
    var depth func(node *TreeNode) int

    depth = func(node *TreeNode) int {
        // Base case: if the node is null, the depth is 0
        if node == nil {
            return 0
        }

        // Recurse to find the depths of left and right subtrees
        leftDepth := depth(node.Left)
        rightDepth := depth(node.Right)

        // Current diameter at this node is the sum of heights of left and right subtrees
        maxDiameter = max(maxDiameter, leftDepth+rightDepth)

        // Return the depth of this node, which is max of left/right subtree depth + 1 (for this node)
        return 1 + max(leftDepth, rightDepth)
    }

    depth(root)
    return maxDiameter
}

// max returns the maximum of two integers.
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```
### Time Complexity:
- **O(n)**: Every node of the tree is visited once.

### Space Complexity:
- **O(n)**: Space for the recursive stack can go as deep as the height of the tree, in the worst case it could be O(n) for a skewed tree.

## Approach 2: Optimized Recursive DFS with Global Variable

### Intuition:
While the previous approach sets `maxDiameter` inside a helper function, it would be more straightforward to maintain the maximum diameter as a global variable or as a reference that gets updated.

### Code:
```go
// TreeNode represents a single node in a binary tree.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func diameterOfBinaryTree(root *TreeNode) int {
    var maxDiameter int

    // Depth-first search that calculates depth and updates maxDiameter globally.
    var dfs func(node *TreeNode) int
    dfs = func(node *TreeNode) int {
        if node == nil {
            return 0
        }

        leftDepth := dfs(node.Left)
        rightDepth := dfs(node.Right)
        
        // Update the diameter if the path through this node is the largest seen so far.
        currentDiameter := leftDepth + rightDepth
        if currentDiameter > maxDiameter {
            maxDiameter = currentDiameter
        }

        // Return height of this node
        return 1 + max(leftDepth, rightDepth)
    }

    dfs(root)
    return maxDiameter
}
```
### Time Complexity:
- **O(n)**: Every node is visited once, same as before.

### Space Complexity:
- **O(h)**: Depends on the height of the tree due to DFS recursion stack, where `h` is the height of the tree. In the worst case, `h = n` (a completely skewed tree).

