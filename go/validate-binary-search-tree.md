# [LeetCode 98: Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Table of Contents
1. [Approach 1: Recursion with Valid Range Checking](#approach-1)
2. [Approach 2: In-order Traversal](#approach-2)
3. [Approach 3: Iterative In-order Traversal with Stack](#approach-3)

---

## Approach 1: Recursion with Valid Range Checking <a name="approach-1"></a>

### Intuition:
The simplest way to validate a binary search tree is to ensure that every node satisfies the constraints of the BST property:
1. All nodes in the left subtree of a node must be less than the node's value.
2. All nodes in the right subtree of a node must be more than the node's value.
3. Apply these constraints recursively by passing down the allowable `min` and `max` range for each subtree.

### Code:
```go
package main

import (
	"math"
)

// TreeNode represents a binary tree node.
type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func isValidBST(root *TreeNode) bool {
	return validate(root, math.MinInt64, math.MaxInt64)
}

func validate(node *TreeNode, min, max int) bool {
	if node == nil {
		return true
	}
	if node.Val <= min || node.Val >= max {
		return false
	}
	// Validate recursively with updated constraints for left and right subtrees.
	return validate(node.Left, min, node.Val) && validate(node.Right, node.Val, max)
}
```

### Complexity:
- **Time Complexity**: O(n), where n is the number of nodes in the tree because we visit each node exactly once.
- **Space Complexity**: O(h), where h is the height of the tree due to the recursive stack.

---

## Approach 2: In-order Traversal <a name="approach-2"></a>

### Intuition:
In a BST, an in-order traversal will visit the nodes in increasingly sorted order. By performing an in-order traversal and checking if each visited node value is greater than the previous, we can validate the BST property.

### Code:
```go
package main

import (
	"math"
)

func isValidBST(root *TreeNode) bool {
	inOrderValues := []int{}
	inOrder(root, &inOrderValues)

	for i := 1; i < len(inOrderValues); i++ {
		if inOrderValues[i] <= inOrderValues[i-1] {
			return false
		}
	}

	return true
}

func inOrder(node *TreeNode, values *[]int) {
	if node == nil {
		return
	}

	// Visit the left subtree.
	inOrder(node.Left, values)

	// Visit the current node.
	*values = append(*values, node.Val)

	// Visit the right subtree.
	inOrder(node.Right, values)
}
```

### Complexity:
- **Time Complexity**: O(n), where n is the number of nodes since each node is visited once.
- **Space Complexity**: O(n), for storing in-order traversal result.

---

## Approach 3: Iterative In-order Traversal with Stack <a name="approach-3"></a>

### Intuition:
The problem can also be solved without recursion using an explicit stack to simulate the in-order traversal. Maintain a `prev` variable to track the previously visited node value and ensure the current node value is greater.

### Code:
```go
package main

func isValidBST(root *TreeNode) bool {
	stack := []*TreeNode{}
	prev := math.MinInt64

	cur := root
	for cur != nil || len(stack) > 0 {
		// Reach the leftmost node.
		for cur != nil {
			stack = append(stack, cur)
			cur = cur.Left
		}

		// Pop the node from the stack, and visit it.
		cur = stack[len(stack)-1]
		stack = stack[:len(stack)-1]

		// Ensure current value is greater than previous value.
		if cur.Val <= prev {
			return false
		}
		prev = cur.Val

		// Visit the right subtree.
		cur = cur.Right
	}
	return true
}
```

### Complexity:
- **Time Complexity**: O(n), since every node is visited once.
- **Space Complexity**: O(h), where h is the height of the tree due to the stack.

