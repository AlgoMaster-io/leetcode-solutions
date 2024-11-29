# 297. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

## Approach 1: BFS (Level Order Traversal) Using Queue

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the serialized string and queue
package main

import (
	"fmt"
	"strings"
	"strconv"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

type Codec struct{}

func Constructor() Codec {
	return Codec{}
}

// Encodes a tree to a single string
func (this *Codec) serialize(root *TreeNode) string {
	if root == nil {
		return "null" // Return "null" for an empty tree
	}

	var serialized strings.Builder
	queue := []*TreeNode{root}

	for len(queue) > 0 {
		current := queue[0]
		queue = queue[1:]

		if current == nil {
			serialized.WriteString("null,")
		} else {
			serialized.WriteString(fmt.Sprintf("%d,", current.Val))
			queue = append(queue, current.Left)
			queue = append(queue, current.Right)
		}
	}

	return serialized.String()
}

// Decodes your encoded data to tree
func (this *Codec) deserialize(data string) *TreeNode {
	if data == "null" {
		return nil // Return nil for an empty tree
	}

	nodes := strings.Split(data, ",")
	rootVal, _ := strconv.Atoi(nodes[0])
	root := &TreeNode{Val: rootVal}
	queue := []*TreeNode{root}

	i := 1
	for len(queue) > 0 && i < len(nodes) {
		current := queue[0]
		queue = queue[1:]

		if nodes[i] != "null" {
			leftVal, _ := strconv.Atoi(nodes[i])
			current.Left = &TreeNode{Val: leftVal}
			queue = append(queue, current.Left)
		}
		i++

		if i < len(nodes) && nodes[i] != "null" {
			rightVal, _ := strconv.Atoi(nodes[i])
			current.Right = &TreeNode{Val: rightVal}
			queue = append(queue, current.Right)
		}
		i++
	}

	return root
}
```

## Approach 2: DFS (Preorder Traversal) Using Recursion

### Solution
```go
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for recursion stack and serialized string
package main

import (
	"strings"
	"strconv"
)

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

type Codec struct{}

func Constructor() Codec {
	return Codec{}
}

// Encodes a tree to a single string
func (this *Codec) serialize(root *TreeNode) string {
	if root == nil {
		return "null" // Return "null" for an empty tree
	}

	var serialized strings.Builder
	this.serializeHelper(root, &serialized)
	return serialized.String()
}

func (this *Codec) serializeHelper(node *TreeNode, serialized *strings.Builder) {
	if node == nil {
		serialized.WriteString("null,")
		return
	}

	serialized.WriteString(strconv.Itoa(node.Val) + ",")
	this.serializeHelper(node.Left, serialized)
	this.serializeHelper(node.Right, serialized)
}

// Decodes your encoded data to tree
func (this *Codec) deserialize(data string) *TreeNode {
	nodes := strings.Split(data, ",")
	queue := []string{}
	queue = append(queue, nodes...)
	return this.deserializeHelper(&queue)
}

func (this *Codec) deserializeHelper(queue *[]string) *TreeNode {
	current := (*queue)[0]
	*queue = (*queue)[1:] // dequeue

	if current == "null" {
		return nil // Return nil for "null" nodes
	}

	val, _ := strconv.Atoi(current)
	node := &TreeNode{Val: val}
	node.Left = this.deserializeHelper(queue)
	node.Right = this.deserializeHelper(queue)

	return node
}
```

