# [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Iterative Approach using Two Stacks](#iterative-approach-using-two-stacks)
- [Iterative Approach using One Stack and Reverse Mechanism](#iterative-approach-using-one-stack-and-reverse-mechanism)

### Recursive Approach

#### Intuition
The postorder traversal of a binary tree involves visiting the left subtree, then the right subtree, and finally the node itself. A natural way to implement this is through recursion, where you first recursively perform a postorder traversal on the left child, then on the right child, and finally process the current node.

#### Approach
1. Define a helper function that takes a node and a result list.
2. If the node is not `nil`, recursively call the helper on the left child.
3. Call the helper on the right child.
4. Append the node's value to the result list after both recursive calls.
5. Start the recursion with the root of the tree.

#### Time Complexity
- **O(n)**, where `n` is the number of nodes in the tree, because we visit each node once.

#### Space Complexity
- **O(n)**, the space complexity is determined by the maximum depth of the call stack, which is `O(h)` where `h` is the height of the tree. In the worst case, for a skewed tree, it could be `O(n)`.

```go
package main

import "fmt"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func postorderTraversal(root *TreeNode) []int {
    var result []int
    postorder(root, &result)
    return result
}

func postorder(node *TreeNode, result *[]int) {
    // If the current node is nil, return immediately
    if node == nil {
        return
    }
    // Recurse on the left subtree
    postorder(node.Left, result)
    // Recurse on the right subtree
    postorder(node.Right, result)
    // Add the value of the current node to the result
    *result = append(*result, node.Val)
}

func main() {
    // Assume a test binary tree is created here
}
```

### Iterative Approach using Two Stacks

#### Intuition
An iterative approach to simulate the recursive process traditionally uses two stacks. The idea is to use one stack to traverse the tree similarly to a preorder traversal, but instead of processing the node in the first visit, you push it to a second stack which will eventually store the nodes in reverse postorder.

#### Approach
1. Initialize two stacks, `stack1` for traversal and `stack2` for storage.
2. Push the root to `stack1`.
3. While `stack1` is not empty, pop from `stack1`, push the node's value to `stack2`.
4. Push the node's left child, then the right child to `stack1` to ensure right child is processed after the left.
5. After the traversal, `stack2` contains the nodes in reverse postorder, so append them to the result in reverse order.

#### Time Complexity
- **O(n)**, similar to the recursive method.

#### Space Complexity
- **O(n)**, for the extra space used by two stacks.

```go
func postorderTraversalIterativeTwoStacks(root *TreeNode) []int {
    if root == nil {
        return nil
    }

    var result []int
    stack1 := []*TreeNode{}
    stack2 := []*TreeNode{}
    stack1 = append(stack1, root)
    
    // While there is something in stack1, keep processing
    for len(stack1) > 0 {
        // Pop the top node from stack1
        node := stack1[len(stack1)-1]
        stack1 = stack1[:len(stack1)-1]
        
        // Push the node into stack2 to store it in "reverse postorder"
        stack2 = append(stack2, node)
        
        // Push left and then right children of the popped node into stack1
        if node.Left != nil {
            stack1 = append(stack1, node.Left)
        }
        if node.Right != nil {
            stack1 = append(stack1, node.Right)
        }
    }
    
    // Pop nodes from stack2 and append their values to result to complete postorder
    for len(stack2) > 0 {
        node := stack2[len(stack2)-1]
        stack2 = stack2[:len(stack2)-1]
        result = append(result, node.Val)
    }
    
    return result
}
```

### Iterative Approach using One Stack and Reverse Mechanism

#### Intuition
We can also achieve the same traversal using only one stack by modifying the sequence of traversals. This can be done by reversing the order of target sequence directions (i.e., right before left), which when reversed later, gives us left-to-right postorder.
  
#### Approach
1. Use a single stack to hold nodes and a tag for whether we are visiting a node for the first time.
2. Tag each node with a boolean indicating whether it has been fully processed.
3. Process the node in a (Root, Right, Left) order, very much like a mirrored preorder traversal.
4. Reverse the result list at the end to get the desired (Left, Right, Root) order.

#### Time Complexity
- **O(n)**, since every node is visited once.

#### Space Complexity
- **O(n)**, mostly due to the stack storing nodes in memory.

```go
func postorderTraversalIterativeOneStack(root *TreeNode) []int {
    if root == nil {
        return nil
    }
    
    result := []int{}
    stack := []*TreeNode{root}
    
    for len(stack) > 0 {
        // Simulate the reverse of postorder by treating it as a modified pre-order
        node := stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        
        // Prepend node's value to the beginning of result list
        result = append([]int{node.Val}, result...)
        
        // Push left child first, so that right child is processed first
        if node.Left != nil {
            stack = append(stack, node.Left)
        }
        if node.Right != nil {
            stack = append(stack, node.Right)
        }
    }
    
    return result
}
```

These three approaches offer a comprehensive solution to the problem of binary tree postorder traversal using recursive and iterative methods.

