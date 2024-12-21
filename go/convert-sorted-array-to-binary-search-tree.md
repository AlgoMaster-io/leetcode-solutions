# [Convert Sorted Array to Binary Search Tree - LeetCode](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

## Navigation
- [Approach 1: Simple Recursive Solution](#approach-1-simple-recursive-solution)
- [Approach 2: Optimized Recursive Solution](#approach-2-optimized-recursive-solution)

## Approach 1: Simple Recursive Solution

### Intuition
To convert a sorted array into a balanced binary search tree, we can utilize the properties of a BST and the fact that the array is sorted:
1. The middle element of the array should become the root of the BST (or subtree) because elements to its left are smaller and elements to its right are larger. 
2. Recursively apply the same logic for the left and right subarrays to build the left and right subtrees.

This approach takes advantage of recursive subdivision of the array, effectively creating a balanced tree.

### Implementation in Go
```go
/**
 * Definition for TreeNode.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func sortedArrayToBST(nums []int) *TreeNode {
    if len(nums) == 0 {
        return nil
    }
    // Find the middle index
    mid := len(nums) / 2
    // Create the current node
    root := &TreeNode{
        Val: nums[mid],
    }
    // Recursively construct the left and right subtrees
    root.Left = sortedArrayToBST(nums[:mid])
    root.Right = sortedArrayToBST(nums[mid+1:])
    return root
}
```

### Time Complexity
- **O(n)**: We need to process each element in the array once.

### Space Complexity
- **O(log n)**: This is the space used by the recursion stack in the average and best cases, where the tree is balanced. In the worst case, this could go up to O(n).

## Approach 2: Optimized Recursive Solution

### Intuition
The simple recursive solution works well, but it keeps creating new subarray slices which can be inefficient in terms of space. Instead, we can pass indices to represent subarrays, thus avoiding additional space overhead from slicing arrays.

### Implementation in Go
```go
func sortedArrayToBSTOptimized(nums []int) *TreeNode {
    return buildBST(nums, 0, len(nums) - 1)
}

// Helper function to construct BST using start and end indices
func buildBST(nums []int, start, end int) *TreeNode {
    // Base case: when the start is greater than end, no more elements to process
    if start > end {
        return nil
    }
    // Find the middle index of the current subarray
    mid := (start + end) / 2
    // Create the current node
    node := &TreeNode{
        Val: nums[mid],
    }
    // Recursively construct the left and right subtrees
    node.Left = buildBST(nums, start, mid-1)
    node.Right = buildBST(nums, mid+1, end)
    return node
}
```

### Time Complexity
- **O(n)**: The same as before, since we still process each element once.

### Space Complexity
- **O(log n)**: By avoiding array slices, we retain the efficient use of space through recursive calls.

