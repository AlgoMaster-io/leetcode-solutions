## [Leetcode 102: Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

### Approaches:
- [Approach 1: Recursive Level Order Traversal](#approach-1-recursive-level-order-traversal)
- [Approach 2: Iterative Level Order Traversal using Queue](#approach-2-iterative-level-order-traversal-using-queue)

---

### Approach 1: Recursive Level Order Traversal

#### Intuition:
The idea is to traverse the binary tree level by level and collect nodes at each depth into separate lists. This can be done recursively by using a helper function that will track the current depth and add nodes to sublists based on their level.

1. **Base Case:** If the node is `nil`, return as there is nothing to add.
2. **Recursive Case:** If the currently tracked depth exceeds the number of lists in our `result` container, add a new list for this level.
3. For each node, add its value to the corresponding level list and recursively traverse its left and right children, increasing its depth.

#### Solution Code:

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func levelOrder(root *TreeNode) [][]int {
	result := [][]int{}
	helper(root, 0, &result)
	return result
}

func helper(node *TreeNode, depth int, result *[][]int) {
	if node == nil {
		return
	}
	// If we don't have a list for this level yet, create it
	if len(*result) == depth {
		*result = append(*result, []int{})
	}
	// Add the current node value to its corresponding depth level
	(*result)[depth] = append((*result)[depth], node.Val)
	// Recursively visit the left and right children, with increased depth
	helper(node.Left, depth+1, result)
	helper(node.Right, depth+1, result)
}
```

#### Time Complexity:
- O(n), where n is the number of nodes in the tree. We visit each node exactly once.

#### Space Complexity:
- O(h), where h is the height of the tree. This accounts for the recursion stack space.

---

### Approach 2: Iterative Level Order Traversal using Queue

#### Intuition:
An iterative approach can be implemented using a queue (FIFO structure) to hold nodes at the current level. The core idea is to use the queue to keep track of nodes to be processed. 

1. **Initialization:** Start by placing the root node in the queue.
2. **Processing:** While the queue is not empty, for each level:
   - Determine the number of nodes at the current level.
   - Dequeue nodes, collect their values, and enqueue their children.
3. Collect the values level by level.

This approach simulates the breadth-first search (BFS) traversal.

#### Solution Code:

```go
func levelOrder(root *TreeNode) [][]int {
	if root == nil {
		return [][]int{}
	}
	
	var result [][]int
	queue := []*TreeNode{root}
	
	for len(queue) > 0 {
		var level []int
		levelSize := len(queue)
		
		// Process all nodes at the current level
		for i := 0; i < levelSize; i++ {
			// Dequeue the front node
			node := queue[0]
			queue = queue[1:] 
			
			// Append its value to the level list
			level = append(level, node.Val)
			
			// Enqueue the left and right children if they exist
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
		}
		// Add the level list to the result
		result = append(result, level)
	}
	
	return result
}
```

#### Time Complexity:
- O(n), where n is the number of nodes in the tree. We process each node exactly once.

#### Space Complexity:
- O(n), where n is the number of nodes in the tree. In the worst case, the last level could hold about n/2 nodes.

