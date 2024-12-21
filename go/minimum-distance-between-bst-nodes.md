# [Leetcode 783: Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

## Approaches
- [Approach 1: Inorder Traversal with List](#approach-1-inorder-traversal-with-list)
- [Approach 2: Inorder Traversal with Constant Space](#approach-2-inorder-traversal-with-constant-space)

### Approach 1: Inorder Traversal with List

#### Intuition
A Binary Search Tree (BST) is a binary tree where the left subtree of a node contains only nodes with keys lesser than the node’s key, and the right subtree only nodes with keys greater. An inorder traversal of a BST visits the nodes in the ascending order of their values. By collecting these node values into a list during inorder traversal, we can easily calculate the minimum difference between consecutive elements in this sorted list.

#### Steps
1. Perform an inorder traversal of the BST.
2. Collect all node values in a sorted list.
3. Iterate through the list and calculate the minimum difference between consecutive elements.

#### Time Complexity
- **Time Complexity**: O(N) where N is the number of nodes; we visit each node once during inorder traversal.
- **Space Complexity**: O(N) to store node values in a list.

```go
func minDiffInBST(root *TreeNode) int {
    values := []int{}
    
    // Helper function to perform inorder traversal and collect values
    var inorder func(node *TreeNode)
    inorder = func(node *TreeNode) {
        if node == nil {
            return
        }
        inorder(node.Left)
        values = append(values, node.Val)
        inorder(node.Right)
    }
    
    inorder(root)
    
    // Calculate the minimum difference in the sorted list
    minDiff := int(^uint(0) >> 1) // Initialize with max int value
    for i := 1; i < len(values); i++ {
        diff := values[i] - values[i-1]
        if diff < minDiff {
            minDiff = diff
        }
    }
    
    return minDiff
}
```

### Approach 2: Inorder Traversal with Constant Space

#### Intuition
To optimize the space complexity, we will use a variable to store the last visited node value during inorder traversal instead of storing all node values. This way, we calculate the difference between the current node value and the last visited node value immediately, and update the minimum difference on the fly.

#### Steps
1. Use an inorder traversal to visit nodes in ascending order.
2. Maintain a variable for the last visited node value and update it during traversal.
3. Calculate the difference with the current node value and keep track of the minimum difference.

#### Time Complexity
- **Time Complexity**: O(N) where N is the number of nodes in the BST.
- **Space Complexity**: O(1) if the recursion stack is not considered, otherwise O(H) where H is the height of the tree due to recursion stack.

```go
func minDiffInBST(root *TreeNode) int {
    var previous *int
    minDiff := int(^uint(0) >> 1) // Initialize with max int value
    
    // Helper function to perform inorder traversal and calculate min difference
    var inorder func(node *TreeNode)
    inorder = func(node *TreeNode) {
        if node == nil {
            return
        }
        
        inorder(node.Left)
        
        // Calculate difference with previous node if it exists
        if previous != nil {
            diff := node.Val - *previous
            if diff < minDiff {
                minDiff = diff
            }
        }
        
        // Update the last visited value
        previous = &node.Val
        
        inorder(node.Right)
    }
    
    inorder(root)
    
    return minDiff
}
```
For both approaches, the inorder traversal respects the properties of the BST which allows us to efficiently find the minimum difference by processing elements in sorted order.

