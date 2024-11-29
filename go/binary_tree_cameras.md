# 968. [Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approach 1: Greedy Approach with Recursion

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(h) where h is the height of the tree (recursion stack)
package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func minCameraCover(root *TreeNode) int {
    cameras := 0
    if dfs(root, &cameras) == 0 {
        cameras++
    }
    return cameras
}

func dfs(node *TreeNode, cameras *int) int {
    if node == nil {
        return 1 // This node is covered
    }

    left := dfs(node.Left, cameras)
    right := dfs(node.Right, cameras)

    if left == 0 || right == 0 {
        *cameras++
        return 2 // Placing a camera at this node
    }

    if left == 2 || right == 2 {
        return 1 // This node is covered
    }

    return 0 // This node needs a camera
}
```

## Approach 2: Dynamic Programming with State Tracking

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

const (
    HAS_CAMERA    = 0
    COVERED       = 1
    NEEDS_CAMERA  = 2
)

func minCameraCover(root *TreeNode) int {
    dp := make(map[*TreeNode][]int)
    if dfs(root, dp) == NEEDS_CAMERA {
        return dp[root][HAS_CAMERA] + 1
    }
    return dp[root][HAS_CAMERA]
}

func dfs(node *TreeNode, dp map[*TreeNode][]int) int {
    if node == nil {
        return COVERED
    }

    if _, ok := dp[node]; !ok {
        dp[node] = make([]int, 3)

        left := dfs(node.Left, dp)
        right := dfs(node.Right, dp)

        if left == NEEDS_CAMERA || right == NEEDS_CAMERA {
            dp[node][HAS_CAMERA] = dpDefault(node, dp)[HAS_CAMERA] + 1
            return HAS_CAMERA
        }

        if left == HAS_CAMERA || right == HAS_CAMERA {
            dp[node][COVERED] = dpDefault(node, dp)[COVERED]
            return COVERED
        }

        dp[node][NEEDS_CAMERA] = dpDefault(node, dp)[NEEDS_CAMERA]
        return NEEDS_CAMERA
    }

    return dp[node][0]
}

func dpDefault(node *TreeNode, dp map[*TreeNode][]int) []int {
    if v, ok := dp[node]; ok {
        return v
    }
    return []int{0, 0, 0}
}
```

Note: The second approach uses dynamic programming concepts, involving a detailed state management via a map, but the greedy recursive version is often more straightforward and optimized for this specific problem.

