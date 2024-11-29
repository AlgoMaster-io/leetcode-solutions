# 230. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

## Approach 1: Inorder Traversal (Recursive)

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree (in the worst case)
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
package main

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

type Solution struct {
	count  int
	result int
}

func (s *Solution) kthSmallest(root *TreeNode, k int) int {
	s.inorder(root, k)
	return s.result
}

func (s *Solution) inorder(node *TreeNode, k int) {
	if node == nil {
		return
	}

	s.inorder(node.Left, k)

	s.count++
	if s.count == k {
		s.result = node.Val
		return
	}

	s.inorder(node.Right, k)
}
```

## Approach 2: Inorder Traversal (Iterative)

### Solution
go
```go
// Time Complexity: O(k), where k is the kth smallest element to be found
// Space Complexity: O(h), where h is the height of the tree for the stack
package main

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func kthSmallest(root *TreeNode, k int) int {
	stack := []*TreeNode{}
	current := root
	count := 0

	for current != nil || len(stack) > 0 {
		for current != nil {
			stack = append(stack, current)
			current = current.Left
		}
		
		current = stack[len(stack)-1]
		stack = stack[:len(stack)-1]
		count++

		if count == k {
			return current.Val
		}

		current = current.Right
	}

	return -1 // This line should never be reached if k is valid
}
```

## Approach 3: Binary Search on BST Property

### Solution
go
```go
// Time Complexity: O(h + k), where h is the height of the tree and k is the position of the kth smallest element
// Space Complexity: O(h), where h is the height of the tree for recursion stack
package main

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func kthSmallest(root *TreeNode, k int) int {
	leftNodeCount := countNodes(root.Left)

	if leftNodeCount == k-1 {
		return root.Val
	} else if leftNodeCount >= k {
		return kthSmallest(root.Left, k)
	} else {
		return kthSmallest(root.Right, k-leftNodeCount-1)
	}
}

func countNodes(node *TreeNode) int {
	if node == nil {
		return 0
	}
	return 1 + countNodes(node.Left) + countNodes(node.Right)
}
```


