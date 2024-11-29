# [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Approach 1: Recursive Construction

### Solution
go
```go
// Time Complexity: O(n^2) in the worst case (unbalanced tree) and O(n log n) on average
// Space Complexity: O(n) (due to recursion stack)

package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func constructMaximumBinaryTree(nums []int) *TreeNode {
    return buildTree(nums, 0, len(nums)-1)
}

func buildTree(nums []int, left, right int) *TreeNode {
    if left > right {
        return nil // Base case: no elements in the range
    }

    // Find the index of the maximum element in the current range
    maxIndex := left
    for i := left + 1; i <= right; i++ {
        if nums[i] > nums[maxIndex] {
            maxIndex = i
        }
    }

    // Create the root node with the maximum value
    root := &TreeNode{Val: nums[maxIndex]}

    // Recursively construct the left and right subtrees
    root.Left = buildTree(nums, left, maxIndex-1)
    root.Right = buildTree(nums, maxIndex+1, right)

    return root
}
```

## Approach 2: Monotonic Stack (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)

package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func constructMaximumBinaryTree(nums []int) *TreeNode {
    stack := []*TreeNode{}

    for _, num := range nums {
        current := &TreeNode{Val: num}

        // Maintain the stack such that the current node is the parent
        // of the last node in the stack if it has a smaller value
        for len(stack) > 0 && stack[len(stack)-1].Val < num {
            current.Left = stack[len(stack)-1]
            stack = stack[:len(stack)-1]
        }

        // The last node in the stack becomes the right child of the current node
        if len(stack) > 0 {
            stack[len(stack)-1].Right = current
        }

        // Push the current node onto the stack
        stack = append(stack, current)
    }

    // The root of the tree is the bottom-most node in the stack
    return stack[0]
}
```

