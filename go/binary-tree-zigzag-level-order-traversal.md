# [Leetcode 103: Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Approaches
1. [Breadth-First Search (BFS) with Queue and Stack](#approach-1-bfs-with-queue-and-stack)
2. [BFS with Level Tracking](#approach-2-bfs-with-level-tracking)

---

## Approach 1: BFS with Queue and Stack
This approach utilizes a queue for level order traversal and a stack for reversing the order of nodes when required.

### Intuition
The main idea is to perform a regular level order traversal using a queue. The special condition here is to reverse the order of nodes at alternate levels. A stack data structure can efficiently collect nodes in the current level and reverse their order before adding them to the final result.

### Steps
1. **Initialize a Queue**: Start with the root node in the queue for the level order traversal.
2. **Level Order Traversal**: For each level, traverse all nodes, collect their values, and add their children to the queue.
3. **Reverse According to Level**: If the current level is odd (1, 3, 5,...), reverse the node values collected.
4. **Compile Result**: Keep appending the processed level nodes to the final result.

### Code
```go
// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func zigzagLevelOrder(root *TreeNode) [][]int {
    if root == nil {
        return [][]int{}
    }

    result := [][]int{}
    queue := []*TreeNode{root}
    direction := 1 // 1 for left to right, -1 for right to left
    
    for len(queue) > 0 {
        levelSize := len(queue)
        level := []int{}
        
        for i := 0; i < levelSize; i++ {
            current := queue[0]
            queue = queue[1:]
            level = append(level, current.Val)
            
            if current.Left != nil {
                queue = append(queue, current.Left)
            }
            if current.Right != nil {
                queue = append(queue, current.Right)
            }
        }
        
        if direction == -1 {
            reverse(level)
        }
        result = append(result, level)
        
        direction *= -1
    }
    
    return result
}

func reverse(arr []int) {
    left, right := 0, len(arr)-1
    for left < right {
        arr[left], arr[right] = arr[right], arr[left]
        left++
        right--
    }
}
```
### Time Complexity
- O(n): Each node is visited exactly once.
### Space Complexity
- O(n): Space needed for the result and the queue, in the worst case for a complete binary tree.

---

## Approach 2: BFS with Level Tracking
This approach modifies the BFS implementation slightly by keeping track of levels and collecting nodes for each level directly.

### Intuition
Instead of using a separate stack for reversing, we keep track of the current level and directly append nodes for even levels while prepending for odd levels.

### Steps
1. **Level Information**: Maintain a separate list for each level while traversing the nodes.
2. **Direction Handling**: Directly prepend or append nodes to the respective list based on the level index.
3. **Construct Levels**: Compile the levels into the result instead of reversing afterwards.

### Code
```go
func zigzagLevelOrder2(root *TreeNode) [][]int {
    if root == nil {
        return [][]int{}
    }
    
    result := [][]int{}
    queue := []*TreeNode{root}
    level := 0
    
    for len(queue) > 0 {
        levelSize := len(queue)
        currentLevel := []int{}
        
        for i := 0; i < levelSize; i++ {
            current := queue[0]
            queue = queue[1:]
            
            if level%2 == 0 {
                currentLevel = append(currentLevel, current.Val)
            } else {
                currentLevel = append([]int{current.Val}, currentLevel...)
            }
            
            if current.Left != nil {
                queue = append(queue, current.Left)
            }
            if current.Right != nil {
                queue = append(queue, current.Right)
            }
        }
        
        result = append(result, currentLevel)
        level++
    }
    
    return result
}
```
### Time Complexity
- O(n): Each node is processed once.
### Space Complexity
- O(n): Storing nodes in the queue and final result array for each level.

