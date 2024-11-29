# 337. [House Robber III](https://leetcode.com/problems/house-robber-iii/)

## Approach 1: Recursive Approach with Memoization

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

import (
    "fmt"
)

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func rob(root *TreeNode) int {
    memo := make(map[*TreeNode]int)
    var robSub func(node *TreeNode) int
    robSub = func(node *TreeNode) int {
        if node == nil {
            return 0
        }
        if val, found := memo[node]; found {
            return val
        }

        // if we rob this root, we cannot rob its direct children
        robRoot := node.Val
        if node.Left != nil {
            robRoot += robSub(node.Left.Left) + robSub(node.Left.Right)
        }
        if node.Right != nil {
            robRoot += robSub(node.Right.Left) + robSub(node.Right.Right)
        }

        // if we do not rob this root, we are free to rob its children
        skipRoot := robSub(node.Left) + robSub(node.Right)

        // Choose the maximum money we can rob
        result := max(robRoot, skipRoot)
        memo[node] = result // Memoize the result for current node

        return result
    }
    return robSub(root)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 2: Dynamic Programming with Tree DP

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(h), where h is the height of the tree
package main

import (
    "fmt"
)

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func rob(root *TreeNode) int {
    result := robSub(root)
    return max(result[0], result[1])
}

func robSub(root *TreeNode) [2]int {
    if root == nil {
        return [2]int{0, 0} // Entry for {not rob, rob}
    }

    left := robSub(root.Left)
    right := robSub(root.Right)

    // if we don't rob this node, we can choose to rob or not to rob its children
    notRob := max(left[0], left[1]) + max(right[0], right[1])

    // if we rob this node, we cannot rob its children
    rob := root.Val + left[0] + right[0]

    return [2]int{notRob, rob} // Return array containing {not rob, rob}
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

