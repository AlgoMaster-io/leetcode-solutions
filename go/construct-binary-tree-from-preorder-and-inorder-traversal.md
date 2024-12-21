# [Leetcode 105: Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## Table of Approaches
1. [Recursive Approach with HashMap](#recursive-approach-with-hashmap)
2. [Iterative Approach with a Stack](#iterative-approach-with-a-stack)

### Recursive Approach with HashMap

This solution involves using recursion to build the tree step-by-step. The key observation is that in the preorder traversal, the first element is always the root of the tree. Using this element, we can split the inorder list into left and right subtrees. 

We can leverage a hashmap to quickly locate the root's index in the inorder traversal, saving time from linearly searching through the array repeatedly.

**Intuition:**
- Use the first element in preorder to identify the root.
- Locate index of this root in inorder array (using a hashmap for efficiency).
- The elements to the left of the index in inorder array belong to the left subtree.
- The elements to the right of the index belong to the right subtree.
- Recursively build the left and right subtrees.

```go
package main

import "fmt"

// TreeNode defines the structure of a binary tree node
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

// buildTree constructs binary tree from preorder and inorder traversal using recursion
func buildTree(preorder []int, inorder []int) *TreeNode {
    // Create a hashmap to store value -> index mappings for inorder traversal
    inIndex := make(map[int]int)
    for i, val := range inorder {
        inIndex[val] = i
    }
    
    // Recursive function to build tree 
    var build func(preL, preR, inL, inR int) *TreeNode
    build = func(preL, preR, inL, inR int) *TreeNode {
        if preL > preR {
            return nil
        }
        
        // Preorder's first element is always the root
        rootVal := preorder[preL]
        root := &TreeNode{Val: rootVal}
        
        // Index of the root in inorder array
        inRootIndex := inIndex[rootVal]
        
        // Count of elements in left subtree in inorder array
        leftCount := inRootIndex - inL
        
        // Recursively construct the left and right subtrees
        root.Left = build(preL+1, preL+leftCount, inL, inRootIndex-1)
        root.Right = build(preL+leftCount+1, preR, inRootIndex+1, inR)
        
        return root
    }
    
    return build(0, len(preorder)-1, 0, len(inorder)-1)
}

// Time Complexity: O(n) since we visit each node exactly once and the hashmap allows O(1) access.
// Space Complexity: O(n) due to the hashmap and recursive call stack.

func main() {
    preorder := []int{3, 9, 20, 15, 7}
    inorder := []int{9, 3, 15, 20, 7}
    root := buildTree(preorder, inorder)
    fmt.Println(root.Val) // Output: 3
}
```

### Iterative Approach with a Stack

The iterative approach mimics the recursive stack by using an explicit stack data structure. We make use of the preorder traversal feature where every node visited is the parent node for any upcoming node until the next node should be added to a previously visited node's right child.

**Intuition:**
- Use a stack to simulate the recursive process.
- Track nodes in progress and use preorder to guide tree construction.
- When encountering an inorder value equal to the stack's top, it means we need to finish the current left subtree and move to creating a right subtree.

```go
package main

import "fmt"

// buildTreeIterative constructs binary tree using an iterative method
func buildTreeIterative(preorder []int, inorder []int) *TreeNode {
    if len(preorder) == 0 {
        return nil
    }
    
    root := &TreeNode{Val: preorder[0]}
    stack := []*TreeNode{root}
    inIndex := 0
    
    for i := 1; i < len(preorder); i++ {
        currentNode := stack[len(stack)-1]
        node := &TreeNode{Val: preorder[i]}
        
        // If current node's value in inorder is not equal to
        // the top of the stack, we should continue building the left subtree
        if currentNode.Val != inorder[inIndex] {
            currentNode.Left = node
        } else {
            // Pop the elements from the stack which have completed their left subtree
            for len(stack) != 0 && stack[len(stack)-1].Val == inorder[inIndex] {
                currentNode = stack[len(stack)-1]
                stack = stack[:len(stack)-1]
                inIndex++
            }
            // Assign the newly created node as the right child to currentNode
            currentNode.Right = node
        }
        
        // Always push the newly created node onto the stack
        stack = append(stack, node)
    }
    
    return root
}

// Time Complexity: O(n)
// Space Complexity: O(n) due to the stack storage.

func main() {
    preorder := []int{3, 9, 20, 15, 7}
    inorder := []int{9, 3, 15, 20, 7}
    root := buildTreeIterative(preorder, inorder)
    fmt.Println(root.Val) // Output: 3
}
```

Both solutions effectively reconstruct the binary tree but employ different mechanisms in their approaches. The recursive method is elegant and matches naturally with the definition of trees, while the iterative method better simulates stack behavior in explicit terms.

