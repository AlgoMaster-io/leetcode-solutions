# [Leetcode 530: Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

## Approaches
- [Approach 1: Inorder Traversal with Additional Array](#approach-1-inorder-traversal-with-additional-array)
- [Approach 2: Optimized Inorder Traversal with Constant Space](#approach-2-optimized-inorder-traversal-with-constant-space)

## Approach 1: Inorder Traversal with Additional Array

### Intuition
The inorder traversal of a Binary Search Tree (BST) results in a sorted array of its values. Using this property, we first perform an inorder traversal of the BST to collect the node values in a sorted order. After that, we can simply iterate through this sorted list to find the minimum difference between consecutive elements.

### Steps
1. Perform an inorder traversal of the BST to collect node values in sorted order.
2. Compute the minimum difference by iterating through the sorted values and comparing only consecutive elements.

### Code
```go
/**
 * Definition for a binary tree node.*/
 type TreeNode struct {
     Val int
     Left *TreeNode
     Right *TreeNode
 }

func getMinimumDifference(root *TreeNode) int {
    // Helper function to perform inorder traversal
    var inorder func(node *TreeNode, values *[]int)
    inorder = func(node *TreeNode, values *[]int) {
        if node == nil {
            return
        }
        inorder(node.Left, values)
        *values = append(*values, node.Val)
        inorder(node.Right, values)
    }
    
    values := []int{}
    inorder(root, &values)
    
    minDiff := int(1e9)
    for i := 1; i < len(values); i++ {
        diff := values[i] - values[i-1]
        if diff < minDiff {
            minDiff = diff
        }
    }
    return minDiff
}
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the tree, since we need to visit each node once.
- **Space Complexity**: O(N), for storing the node values during the inorder traversal.

## Approach 2: Optimized Inorder Traversal with Constant Space

### Intuition
We can perform an inorder traversal without maintaining an additional array to store the node values. Instead, we use a variable to keep track of the last visited node's value and compute the difference on-the-fly. This approach reduces the space complexity considerably.

### Steps
1. Use a helper variable `prevNode` initialized to `nil` to remember the last visited node.
2. During the inorder traversal, calculate the difference between the current node value and `prevNode`, updating the minimum difference whenever a smaller one is found.

### Code
```go
func getMinimumDifference(root *TreeNode) int {
    minDiff := int(1e9)
    var prevNode *TreeNode
    var inorder func(node *TreeNode)
    
    inorder = func(node *TreeNode) {
        if node == nil {
            return
        }
        inorder(node.Left)
        if prevNode != nil {
            diff := node.Val - prevNode.Val
            if diff < minDiff {
                minDiff = diff
            }
        }
        prevNode = node
        inorder(node.Right)
    }
    
    inorder(root)
    return minDiff
}
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the tree, as we need to visit each node once.
- **Space Complexity**: O(1), if we ignore the recursion stack, which is effectively O(H) where H is the height of the tree. However, since the recursion stack usage is inevitable, the additional space outside of the recursion stack is constant.

