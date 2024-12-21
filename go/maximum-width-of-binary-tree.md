### [Leetcode 662: Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/)

#### Approaches:
- [Approach 1: Level Order Traversal with Index Tracking (Basic BFS)](#approach-1-level-order-traversal-with-index-tracking-basic-bfs)
- [Approach 2: Optimized BFS with Index Mapping (Optimal BFS with less memory footprint)](#approach-2-optimized-bfs-with-index-mapping-optimal-bfs-with-less-memory-footprint)

---

### Approach 1: Level Order Traversal with Index Tracking (Basic BFS)

#### Intuition:
The idea is to perform a level order traversal (BFS) of the binary tree and keep track of indices of nodes in such a way that each node at the current level can be referred without needing an actual index, thus capturing their maximum width. We maintain the indices by assuming the root node is at index 0, the left child at `2 * index`, and the right child at `2 * index + 1`. By tracking the first and last indices in each level, we can compute the width efficiently.

#### Time and Space Complexity:
- **Time Complexity:** O(N), where N is the number of nodes in the binary tree, since each node is processed once.
- **Space Complexity:** O(N) for storing nodes in the queue.

```go
package main

import "fmt"

// TreeNode definition as per Leetcode.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func widthOfBinaryTree(root *TreeNode) int {
    if root == nil {
        return 0
    }

    maxWidth := 0
    queue := []queueNode{{node: root, index: 0}} // Start BFS with root node

    // Perform BFS
    for len(queue) > 0 {
        n := len(queue)
        firstIndex := queue[0].index
        lastIndex := queue[n-1].index

        // Calculate current level's width
        currentWidth := lastIndex - firstIndex + 1
        if currentWidth > maxWidth {
            maxWidth = currentWidth
        }

        // Process the current level & prepare for the next level
        for i := 0; i < n; i++ {
            currentNode := queue[i]
            currentIndex := currentNode.index

            // Enqueue left child with proper index
            if currentNode.node.Left != nil {
                queue = append(queue, queueNode{
                    node:  currentNode.node.Left,
                    index: 2*currentIndex + 1,
                })
            }
            // Enqueue right child with proper index
            if currentNode.node.Right != nil {
                queue = append(queue, queueNode{
                    node:  currentNode.node.Right,
                    index: 2*currentIndex + 2,
                })
            }
        }
        // Move to the next level
        queue = queue[n:]
    }
    return maxWidth
}

type queueNode struct {
    node  *TreeNode
    index int
}

func main() {
    // Example use-case
    root := &TreeNode{Val: 1}
    root.Left = &TreeNode{Val: 3}
    root.Right = &TreeNode{Val: 2}
    root.Left.Left = &TreeNode{Val: 5}
    root.Left.Right = &TreeNode{Val: 3}
    root.Right.Right = &TreeNode{Val: 9}

    fmt.Println("Maximum width of the binary tree is:", widthOfBinaryTree(root))
}
```

### Approach 2: Optimized BFS with Index Mapping (Optimal BFS with less memory footprint)

#### Intuition:
This approach also employs a level order traversal but aims to reduce the memory usage by avoiding storing full blown indices in the queue. The primary idea remains to track each level's width via indices but using a map to store the first and last index of the node at each level. This method optimizes memory usage while calculating width similar to the previous BFS approach.

#### Time and Space Complexity:
- **Time Complexity:** O(N), where N is the number of nodes in the binary tree.
- **Space Complexity:** O(N) due to queue usage, but with reduced space for indices.

```go
package main

import "fmt"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func widthOfBinaryTree(root *TreeNode) int {
    if root == nil {
        return 0
    }

    maxWidth := 0
    queue := []queueNode{{node: root, index: 0}}
    levelMap := make(map[int]int) // Map to store first index for each level

    for len(queue) > 0 {
        newQueue := []queueNode{}
        for i := range queue {
            currentNode := queue[i].node
            currentIndex := queue[i].index

            // Update level boundaries map for the first appearance
            if _, exists := levelMap[currentIndex]; !exists {
                levelMap[currentIndex] = currentIndex
            }

            // Update maximum width seen so far
            width := currentIndex - levelMap[queue[0].index] + 1
            if width > maxWidth {
                maxWidth = width
            }

            // Enqueue children nodes
            if currentNode.Left != nil {
                newQueue = append(newQueue, queueNode{node: currentNode.Left, index: 2*currentIndex + 1})
            }
            if currentNode.Right != nil {
                newQueue = append(newQueue, queueNode{node: currentNode.Right, index: 2*currentIndex + 2})
            }
        }
        queue = newQueue
    }

    return maxWidth
}

type queueNode struct {
    node  *TreeNode
    index int
}

func main() {
    root := &TreeNode{Val: 1}
    root.Left = &TreeNode{Val: 3}
    root.Right = &TreeNode{Val: 2}
    root.Left.Left = &TreeNode{Val: 5}
    root.Left.Right = &TreeNode{Val: 3}
    root.Right.Right = &TreeNode{Val: 9}

    fmt.Println("Maximum width of the binary tree is:", widthOfBinaryTree(root))
}
```

> The second approach can be utilized if optimization is required for the memory usage, particularly when handling larger binary trees. Both methods effectively compute the maximum width by ensuring we appropriately manage the node indices during traversal.

