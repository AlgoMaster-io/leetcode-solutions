# 257. [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approach 1: Recursive Depth-First Search (DFS)

### Solution

go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
package main

import (
	"strconv"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func binaryTreePaths(root *TreeNode) []string {
	result := []string{}
	if root != nil {
		dfs(root, "", &result)
	}
	return result
}

func dfs(node *TreeNode, path string, result *[]string) {
	// Append the current node's value to the path
	path += strconv.Itoa(node.Val)

	// If the current node is a leaf, add the path to the result
	if node.Left == nil && node.Right == nil {
		*result = append(*result, path)
		return
	}

	// If not a leaf, continue the path with "->" and recurse
	if node.Left != nil {
		dfs(node.Left, path+"->", result)
	}
	if node.Right != nil {
		dfs(node.Right, path+"->", result)
	}
}
```

## Approach 2: Iterative Depth-First Search (Using Stack)

### Solution

go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n), for the stack and path storage
package main

import (
	"strconv"
)

func binaryTreePathsIterative(root *TreeNode) []string {
	result := []string{}
	if root == nil {
		return result // Return empty result if the tree is empty
	}

	stack := []*TreeNode{root}
	paths := []string{strconv.Itoa(root.Val)}

	for len(stack) > 0 {
		currentNode := stack[len(stack)-1]
		stack = stack[:len(stack)-1]

		currentPath := paths[len(paths)-1]
		paths = paths[:len(paths)-1]

		// If the current node is a leaf, add the path to the result
		if currentNode.Left == nil && currentNode.Right == nil {
			result = append(result, currentPath)
		}

		// If not a leaf, push children to the stack with updated paths
		if currentNode.Right != nil {
			stack = append(stack, currentNode.Right)
			paths = append(paths, currentPath+"->"+strconv.Itoa(currentNode.Right.Val))
		}
		if currentNode.Left != nil {
			stack = append(stack, currentNode.Left)
			paths = append(paths, currentPath+"->"+strconv.Itoa(currentNode.Left.Val))
		}
	}

	return result
}
```

## Approach 3: Backtracking

### Solution

go
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
package main

import (
	"strconv"
)

func binaryTreePathsBacktrack(root *TreeNode) []string {
	result := []string{}
	if root == nil {
		return result // Return empty result if the tree is empty
	}
	backtrack(root, "", &result)
	return result
}

func backtrack(node *TreeNode, path string, result *[]string) {
	pathLen := len(path)
	path += strconv.Itoa(node.Val)

	// If the current node is a leaf, add the path to the result
	if node.Left == nil && node.Right == nil {
		*result = append(*result, path)
	} else {
		// If not a leaf, continue the path
		path += "->"
		if node.Left != nil {
			backtrack(node.Left, path, result)
		}
		if node.Right != nil {
			backtrack(node.Right, path, result)
		}
	}
	
	// Backtrack to remove the current node's value and "->"
	path = path[:pathLen]
}
```

