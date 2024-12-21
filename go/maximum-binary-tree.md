# [Leetcode 654: Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Approaches
- [Approach 1: Recursive Construction (Basic)](#approach-1-recursive-construction-basic)
- [Approach 2: Iterative Stack Solution (Optimal)](#approach-2-iterative-stack-solution-optimal)

## Approach 1: Recursive Construction (Basic)

### Intuition
The problem requires constructing a binary tree such that the tree is constructed with a property that each subtree has the root value as the maximum of the given array sub-range. This can be solved recursively by always selecting the maximum element in the array and making it the root of the subtree spanning across that portion of the array. The left subtree is then constructed by recursively applying the same logic to the left segment of the array relative to the maximum, and similarly for the right subtree.

### Steps
1. Find the maximum element in the current array or subarray. This will be the root value.
2. Recursively build the left subtree with the array segment to the left of the maximum value.
3. Recursively build the right subtree with the array segment to the right of the maximum value.
4. Repeat the process until the entire array is traversed.

### Time and Space Complexity
- **Time Complexity**: O(n^2), in the worst case where the array is sorted (either ascending or descending) we will have linear pass for finding max and recursion depth will go till n.
- **Space Complexity**: O(n), due to the recursion stack.

### Go Code
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func constructMaximumBinaryTree(nums []int) *TreeNode {
    return construct(nums, 0, len(nums))
}

func construct(nums []int, left int, right int) *TreeNode {
    if left == right {
        return nil
    }
    
    // Find the index of the maximum value
    maxIndex := left
    for i := left + 1; i < right; i++ {
        if nums[i] > nums[maxIndex] {
            maxIndex = i
        }
    }
    
    // The maximum value becomes the root
    root := &TreeNode{Val: nums[maxIndex]}
    
    // Recursively build the left and right subtrees
    root.Left = construct(nums, left, maxIndex)
    root.Right = construct(nums, maxIndex + 1, right)
    
    return root
}
```

## Approach 2: Iterative Stack Solution (Optimal)

### Intuition
A more optimal solution leverages a monotonic stack that constructs the tree in one pass over the array. As we iterate, we maintain a stack that keeps track of nodes in a decreasing order. While iterating over the array, if the current number is greater than the number represented by the node at the top of the stack, it implies that the current element becomes the parent of the stack’s node and possibly also of other nodes popped from the stack immediately thereafter.

### Steps
1. Iterate over each number from the array.
2. Initialize a new node for each number.
3. Check and pop from the stack until the stack top is greater than the current number, which implies nums[current] is the new parent of these nodes.
4. Set the popped node as the left child of the current number (as it’s smaller), and then the current node becomes its parent.
5. If the stack isn't empty, the current node becomes the right child of the stack's current top (as the stack’s top is still greater).
6. Push the current node onto the stack.

### Time and Space Complexity
- **Time Complexity**: O(n), each element is pushed and popped from the stack once.
- **Space Complexity**: O(n), in the worst case where all elements need to be stored in the stack.

### Go Code
```go
func constructMaximumBinaryTree(nums []int) *TreeNode {
    var stack []*TreeNode
    
    for _, num := range nums {
        current := &TreeNode{Val: num}
        
        // Attach popped nodes as left child if they are smaller
        for len(stack) > 0 && stack[len(stack)-1].Val < num {
            current.Left = stack[len(stack) - 1]
            stack = stack[:len(stack) - 1]
        }
        
        // If there are still nodes in the stack, attach the current node as the right child
        if len(stack) > 0 {
            stack[len(stack) - 1].Right = current
        }
        
        // Push current node to the stack
        stack = append(stack, current)
    }
    
    // The first node pushed in the stack will be the root
    if len(stack) > 0 {
        return stack[0]
    }
    return nil
}
```

