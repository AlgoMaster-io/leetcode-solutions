# [Leetcode 106: Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

## Approaches
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Improved Recursive Approach using Hash Map](#approach-2-improved-recursive-approach-using-hash-map)

### Approach 1: Recursive Approach

#### Intuition
The problem is to construct a binary tree from its inorder and postorder traversals. It's crucial to understand how these traversals depict the structure of the tree. 
- The last element in the postorder traversal represents the root of the current tree (or subtree).
- Locate this root in the inorder traversal; the elements to the left form the left subtree, and the elements to the right form the right subtree.

Using these properties, we can recursively build the tree.

#### Steps
1. Recursively consider the last element of postorder as the root.
2. Find this root in the inorder array.
3. Elements on the left form the left subtree and elements on the right form the right subtree.
4. Recursively repeat the process for each subtree.

Here’s the basic recursive solution:

```go
// Definition for a binary tree node.
type TreeNode struct {
    Val int
    Left *TreeNode
    Right *TreeNode
}

func buildTree(inorder []int, postorder []int) *TreeNode {
    if len(inorder) == 0 || len(postorder) == 0 {
        return nil
    }

    // Get the root value from the end of postorder array
    rootVal := postorder[len(postorder)-1]
    root := &TreeNode{Val: rootVal}

    // Find the index of root in inorder array
    var rootIndex int
    for i, v := range inorder {
        if v == rootVal {
            rootIndex = i
            break
        }
    }

    // Recursively construct the left and right subtrees
    root.Left = buildTree(inorder[:rootIndex], postorder[:rootIndex])
    root.Right = buildTree(inorder[rootIndex+1:], postorder[rootIndex:len(postorder)-1])

    return root
}
```

#### Time Complexity
- Building the tree recursively: O(n^2) in the worst-case where every node is processed to find its index in the inorder array.

#### Space Complexity
- O(n) due to the recursive stack space and the slices.

### Approach 2: Improved Recursive Approach using Hash Map

#### Intuition 
In the previous approach, finding the index of the root in the inorder traversal took O(n) time. We can optimize this by using a hash map to store the index of each value in the inorder traversal, improving the lookup time to O(1).

#### Steps
1. Create a map for inorder traversal with element values as keys and their indices as values.
2. Use this map to quickly find the root's index in the inorder array.
3. Follow the same recursive strategy using indices instead of creating subarrays.

```go
func buildTreeOptimized(inorder []int, postorder []int) *TreeNode {
    inorderMap := make(map[int]int)
    
    // Fill the map with inorder indices
    for i, val := range inorder {
        inorderMap[val] = i
    }

    // Helper function to build the tree
    var helper func(int, int, int, int) *TreeNode
    helper = func(inLeft int, inRight int, postLeft int, postRight int) *TreeNode {
        // base case
        if inLeft > inRight || postLeft > postRight {
            return nil
        }
        
        // Last element in postorder array is the root
        rootVal := postorder[postRight]
        root := &TreeNode{Val: rootVal}

        // root index in inorder array
        index := inorderMap[rootVal]

        // Recursively build left and right subtrees
        root.Left = helper(inLeft, index-1, postLeft, postLeft+index-inLeft-1)
        root.Right = helper(index+1, inRight, postLeft+index-inLeft, postRight-1)
        
        return root
    }
    return helper(0, len(inorder)-1, 0, len(postorder)-1)
}
```

#### Time Complexity
- O(n): Each node is processed a constant number of times.

#### Space Complexity
- O(n): Due to the recursive stack space and hash map for inorder indices.

