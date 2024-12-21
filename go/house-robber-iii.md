# [House Robber III - Leetcode Problem 337](https://leetcode.com/problems/house-robber-iii/)

### Approaches:
- [Recursive Approach with Exponential Time Complexity](#recursive-approach-with-exponential-time-complexity)
- [Recursive Memoization Approach](#recursive-memoization-approach)
- [Optimized Dynamic Programming Approach](#optimized-dynamic-programming-approach)

## Recursive Approach with Exponential Time Complexity
### Intuition:
In this problem, you're essentially tasked with deciding whether or not to rob a particular house (tree node), considering that you can't rob two directly linked houses (parent-child nodes). The naive solution involves a recursion that explores all possibilities, choosing either to rob the current house or skip it.

For each node, you have two choices:
1. Rob this node: In this case, you cannot rob its direct children, so only consider the grandchildren.
2. Skip this node: In this case, you are free to rob its children.

If we consider all possibilities, it becomes apparent that the problem can be broken down into subproblems: solving for the maximum amount of money starting from each node given the two choices above.

### Code:
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func rob(root *TreeNode) int {
    if root == nil {
        return 0
    }

    // Money when robbing the current house
    moneyWhenRobbing := root.Val
    if root.Left != nil {
        moneyWhenRobbing += rob(root.Left.Left) + rob(root.Left.Right)
    }
    if root.Right != nil {
        moneyWhenRobbing += rob(root.Right.Left) + rob(root.Right.Right)
    }

    // Money when skipping the current house
    moneyWhenSkipping := rob(root.Left) + rob(root.Right)

    // Return the maximum of the two approaches
    return max(moneyWhenRobbing, moneyWhenSkipping)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Time and Space Complexity:
- **Time Complexity**: O(2^N), where N is the number of nodes. This is due to the calculation being repeated at each node for each potential rob or skip scenario.
- **Space Complexity**: O(N), related to the call stack size during recursion.

---

## Recursive Memoization Approach
### Intuition:
The above solution has overlapping subproblems, as it calculates the maximum profit for the same nodes multiple times. By introducing a memoization step, we can ensure each node's result is only calculated once and reused for future calculations. This modification drastically improves efficiency by storing previously computed values.

### Code:
```go
func rob(root *TreeNode) int {
    memo := make(map[*TreeNode]int)
    return robWithMemo(root, memo)
}

func robWithMemo(root *TreeNode, memo map[*TreeNode]int) int {
    if root == nil {
        return 0
    }
    if val, ok := memo[root]; ok {
        return val
    }

    moneyWhenRobbing := root.Val
    if root.Left != nil {
        moneyWhenRobbing += robWithMemo(root.Left.Left, memo) + robWithMemo(root.Left.Right, memo)
    }
    if root.Right != nil {
        moneyWhenRobbing += robWithMemo(root.Right.Left, memo) + robWithMemo(root.Right.Right, memo)
    }

    moneyWhenSkipping := robWithMemo(root.Left, memo) + robWithMemo(root.Right, memo)

    result := max(moneyWhenRobbing, moneyWhenSkipping)
    memo[root] = result
    return result
}
```

### Time and Space Complexity:
- **Time Complexity**: O(N), as each node is calculated once and stored.
- **Space Complexity**: O(N), due to memoization storage and recursion stack.

---

## Optimized Dynamic Programming Approach
### Intuition:
The Dynamic Programming approach stores two states for each node: the maximum amount of money you can rob if you rob this node and the maximum you can rob if you do not rob this node. By calculating these two values (using a bottom-up postorder traversal of the tree), you can determine the maximum money obtainable efficiently.

### Code:
```go
func rob(root *TreeNode) int {
    robberies := calculateRobbery(root)
    return max(robberies[0], robberies[1])
}

func calculateRobbery(root *TreeNode) [2]int {
    if root == nil {
        return [2]int{0, 0}
    }

    left := calculateRobbery(root.Left)
    right := calculateRobbery(root.Right)

    robThis := root.Val + left[1] + right[1]
    skipThis := max(left[0], left[1]) + max(right[0], right[1])

    return [2]int{robThis, skipThis}
}
```

### Time and Space Complexity:
- **Time Complexity**: O(N), since we traverse each node once.
- **Space Complexity**: O(H), where H is the height of the tree, due to the recursive stack space.

