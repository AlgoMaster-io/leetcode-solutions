# [Leetcode 173: Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approaches:
1. [Inorder Traversal with Array](#inorder-traversal-with-array)
2. [Controlled Recursion with Stack](#controlled-recursion-with-stack)
3. [Morris Iteration - Advanced Method](#morris-iteration---advanced-method)

---

### Inorder Traversal with Array

**Intuition**: 

The simplest approach is to perform an inorder traversal of the binary search tree (BST) and store the elements in an array. The inorder traversal of a BST yields the nodes in sorted order. Therefore, using an array to store the result of this traversal allows us to easily implement the `next` and `hasNext` methods needed for our iterator.

**Steps**:
1. Traverse the BST in an inorder manner and store the nodes in an array.
2. Use an index to traverse this array for returning the next smallest element.

**Time Complexity**: 
- Initializing the iterator: **O(n)**, where **n** is the number of nodes in the tree, due to the complete inorder traversal.
- Calling `next()`: **O(1)** because it's just array indexing.
- Calling `hasNext()`: **O(1)**

**Space Complexity**: 
- **O(n)** for storing the elements of the tree in an array.

```go
type BSTIterator struct {
    index   int
    inorder []int
}

// Helper function to perform inorder traversal
func inorderTraversal(node *TreeNode, inorder *[]int) {
    if node == nil {
        return
    }
    inorderTraversal(node.Left, inorder)
    *inorder = append(*inorder, node.Val)
    inorderTraversal(node.Right, inorder)
}

func Constructor(root *TreeNode) BSTIterator {
    bst := BSTIterator{
        index:   -1,
        inorder: []int{},
    }
    inorderTraversal(root, &bst.inorder)
    return bst
}

// Returns the next smallest element in the BST
func (this *BSTIterator) Next() int {
    this.index++
    return this.inorder[this.index]
}

// Returns whether we have a next smallest number
func (this *BSTIterator) HasNext() bool {
    return this.index+1 < len(this.inorder)
}
```

---

### Controlled Recursion with Stack

**Intuition**: 

A more optimal solution is to use a controlled recursion approach with a stack to simulate the inorder traversal. Instead of storing all the elements upfront, we dynamically maintain a stack of nodes to track where we are in the traversal.

**Steps**:
1. Initialize a stack and push all the leftmost nodes of the tree to it.
2. For `next()`, pop from the stack, process the node, and push all the leftmost nodes of the popped node's right subtree.
3. For `hasNext()`, simply check if the stack is non-empty.

**Time Complexity**: 
- Initializing the iterator: **O(h)**, where **h** is the height of the tree, as we only push left nodes.
- Calling `next()`: Amortized **O(1)** in a single sequence of throughout traversing.
- Calling `hasNext()`: **O(1)**

**Space Complexity**: 
- **O(h)** at most, where **h** is the height of the tree, due to the stack storage.

```go
type BSTIterator struct {
    stack []*TreeNode
}

// Helper function to push all leftmost nodes of a tree to the stack
func (this *BSTIterator) pushAllLeftNodes(root *TreeNode) {
    for root != nil {
        this.stack = append(this.stack, root)
        root = root.Left
    }
}

func Constructor(root *TreeNode) BSTIterator {
    bst := BSTIterator{stack: []*TreeNode{}}
    bst.pushAllLeftNodes(root)
    return bst
}

// Returns the next smallest element in the BST
func (this *BSTIterator) Next() int {
    node := this.stack[len(this.stack)-1]
    this.stack = this.stack[:len(this.stack)-1] 
    if node.Right != nil {
        this.pushAllLeftNodes(node.Right)
    }
    return node.Val
}

// Returns whether we have a next smallest number
func (this *BSTIterator) HasNext() bool {
    return len(this.stack) > 0
}
```

---

### Morris Iteration - Advanced Method

**Intuition**: 

For completeness, we mention that it's possible (though more complex) to implement an iterator with constant space using Morris traversal. However, this method modifies the tree structure temporarily and isn't practical for an iterator.

**Steps** (Not implemented in code here for clarity):
1. Create threaded links in the tree as you traverse.
2. Use these links to simulate an inorder traversal without recursion or a stack.

**Time Complexity**: 
- **O(n)** upon creating the iterator.
- **O(1)** for both `next()` and `hasNext()`, as each node is processed only once.

**Space Complexity**:
- **O(1)** because no additional data structures are used.

This method is more complex to handle and not typical in practice for such iterators, especially when the stack approach provides efficient and simple solutions without modifying the tree.

