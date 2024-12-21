# [Leetcode 1026: Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/)

## Approaches:

- [Approach 1: DFS Brute Force](#approach-1-dfs-brute-force)
- [Approach 2: Optimized DFS with Min and Max](#approach-2-optimized-dfs-with-min-and-max)

---

## Approach 1: DFS Brute Force

### Intuition

The simplest approach to solve this problem is using a brute force Depth First Search (DFS) strategy. For every node in the tree, we need to find all its ancestors and calculate the difference between the node's value and each ancestor's value. The maximum difference among all nodes should be returned.

### Steps

1. Traverse every node using DFS.
2. For each node, keep a track of all its ancestors by passing them through the recursive call stack.
3. For each node, calculate the differences with its ancestors and update the global maximum difference accordingly.

### Code

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func maxAncestorDiff(root *TreeNode) int {
    if root == nil {
        return 0
    }
    return dfs(root, []int{})
}

func dfs(node *TreeNode, ancestors []int) int {
    if node == nil {
        return 0
    }

    maxDiff := 0
    for _, ancestor := range ancestors {
        diff := abs(node.Val - ancestor)
        if diff > maxDiff {
            maxDiff = diff
        }
    }

    // Add the current node to the ancestor list for child nodes.
    ancestors = append(ancestors, node.Val)

    // Calculate max difference for left and right subtree.
    leftDiff := dfs(node.Left, ancestors)
    rightDiff := dfs(node.Right, ancestors)

    return max(maxDiff, max(leftDiff, rightDiff))
}

func abs(a int) int {
    if a < 0 {
        return -a
    }
    return a
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity Analysis

- **Time Complexity**: O(N^2), as for each node, we iterate through all its ancestors in the recursive stack.
- **Space Complexity**: O(N) for the recursive stack and ancestor tracking.

---

## Approach 2: Optimized DFS with Min and Max

### Intuition

Instead of tracking all ancestors, we can keep track only of the minimum and maximum value encountered from the root to the current node. The maximum difference at any node will be the largest of the differences between the current node's value and the minimum and maximum values encountered.

### Steps

1. Traverse the tree using DFS.
2. Maintain the minimum and maximum values encountered on the path from the root to the current node.
3. At each node, calculate the maximum difference between the node's value and the min/max values.
4. Update the global maximum difference.
5. Recur for left and right subtrees with updated min and max values.

### Code

```go
func maxAncestorDiff(root *TreeNode) int {
    if root == nil {
        return 0
    }
    return dfsOptimized(root, root.Val, root.Val)
}

func dfsOptimized(node *TreeNode, minVal int, maxVal int) int {
    if node == nil {
        return maxVal - minVal
    }

    // Update min and max based on the current node's value.
    if node.Val < minVal {
        minVal = node.Val
    }
    if node.Val > maxVal {
        maxVal = node.Val
    }

    // Recurse for left and right subtrees.
    leftDif := dfsOptimized(node.Left, minVal, maxVal)
    rightDif := dfsOptimized(node.Right, minVal, maxVal)

    return max(leftDif, rightDif)
}
```

### Complexity Analysis

- **Time Complexity**: O(N), since we perform a single traversal of the tree.
- **Space Complexity**: O(H), where H is the height of the tree, due to the recursion stack. In the worst case, H = N for a skewed tree.

This approach is more optimal and efficient as it reduces the time complexity significantly by only keeping track of the essential values needed to compute the maximum difference.

