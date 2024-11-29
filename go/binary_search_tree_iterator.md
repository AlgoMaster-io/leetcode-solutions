# 173. [Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approach 1: Controlled Recursion Using Stack

### Solution
go
```go
// Time Complexity:
//   - Constructor: O(h), where h is the height of the tree
//   - Next(): O(1) on average across all calls
//   - HasNext(): O(1)
// Space Complexity: O(h), where h is the height of the tree for the stack
package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

type BSTIterator struct {
    stack []*TreeNode
}

func Constructor(root *TreeNode) BSTIterator {
    iterator := BSTIterator{stack: []*TreeNode{}}
    iterator.pushLeft(root)
    return iterator
}

func (this *BSTIterator) pushLeft(node *TreeNode) {
    for node != nil {
        this.stack = append(this.stack, node)
        node = node.Left
    }
}

// Next returns the next smallest number
func (this *BSTIterator) Next() int {
    node := this.stack[len(this.stack)-1]
    this.stack = this.stack[:len(this.stack)-1]

    if node.Right != nil {
        this.pushLeft(node.Right)
    }

    return node.Val
}

// HasNext returns whether we have a next smallest number
func (this *BSTIterator) HasNext() bool {
    return len(this.stack) > 0
}
```

## Approach 2: Precomputed Inorder Traversal

### Solution
go
```go
// Time Complexity:
//   - Constructor: O(n), where n is the number of nodes in the tree
//   - Next(): O(1)
//   - HasNext(): O(1)
// Space Complexity: O(n), where n is the number of nodes in the tree
package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

type BSTIterator struct {
    inorder []int
    index   int
}

func Constructor(root *TreeNode) BSTIterator {
    iterator := BSTIterator{inorder: []int{}, index: 0}
    iterator.inorderTraversal(root)
    return iterator
}

func (this *BSTIterator) inorderTraversal(node *TreeNode) {
    if node == nil {
        return
    }
    this.inorderTraversal(node.Left)
    this.inorder = append(this.inorder, node.Val)
    this.inorderTraversal(node.Right)
}

// Next returns the next smallest number
func (this *BSTIterator) Next() int {
    val := this.inorder[this.index]
    this.index++
    return val
}

// HasNext returns whether we have a next smallest number
func (this *BSTIterator) HasNext() bool {
    return this.index < len(this.inorder)
}
```

## Approach 3: Morris Traversal (Space Optimized, Less Practical for Iterator)

### Solution
go
```go
// Time Complexity:
//   - Constructor: O(1) (no precomputation)
//   - Next(): O(1) on average across all calls
//   - HasNext(): O(1)
// Space Complexity: O(1), no additional storage
package main

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

type BSTIterator struct {
    current     *TreeNode
    predecessor *TreeNode
}

func Constructor(root *TreeNode) BSTIterator {
    return BSTIterator{current: root}
}

// Next returns the next smallest number
func (this *BSTIterator) Next() int {
    var value int

    for this.current != nil {
        if this.current.Left == nil {
            value = this.current.Val
            this.current = this.current.Right
            break
        } else {
            this.predecessor = this.current.Left

            for this.predecessor.Right != nil && this.predecessor.Right != this.current {
                this.predecessor = this.predecessor.Right
            }

            if this.predecessor.Right == nil {
                this.predecessor.Right = this.current
                this.current = this.current.Left
            } else {
                this.predecessor.Right = nil
                value = this.current.Val
                this.current = this.current.Right
                break
            }
        }
    }

    return value
}

// HasNext returns whether we have a next smallest number
func (this *BSTIterator) HasNext() bool {
    return this.current != nil
}
```

