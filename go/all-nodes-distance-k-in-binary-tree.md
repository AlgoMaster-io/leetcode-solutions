# [Leetcode 863: All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)

## Approaches
1. [Depth-First Search (DFS) with Parent Pointers](#approach-1-depth-first-search-dfs-with-parent-pointers)
2. [Breadth-First Search (BFS) with Parent Pointers](#approach-2-breadth-first-search-bfs-with-parent-pointers)

---

## Approach 1: Depth-First Search (DFS) with Parent Pointers

### Intuition
To solve the problem, first, we need to transform the tree into something that allows us to move easily from any node to any other node. We can do this by storing parent pointers in a hashmap. Once we have parent pointers, the problem reduces to finding nodes a certain distance in a graph (where each node is connected to its left child, right child, and parent).

Steps:
1. Use DFS to traverse the tree and record the parent of each node.
2. Once we have parent pointers, we can perform a DFS starting from the target node to find all nodes at distance `K`.

### Code
```go
/**
 * Definition for TreeNode.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func distanceK(root, target *TreeNode, K int) []int {
    // Map to store parent pointers for each node
    parent := make(map[*TreeNode]*TreeNode)

    // Helper function to build the parent pointers with DFS
    var buildParentPointers func(node *TreeNode, par *TreeNode)
    buildParentPointers = func(node *TreeNode, par *TreeNode) {
        if node == nil {
            return
        }
        parent[node] = par
        buildParentPointers(node.Left, node)
        buildParentPointers(node.Right, node)
    }

    buildParentPointers(root, nil)

    // Result to store nodes at distance K
    var result []int
    visited := make(map[*TreeNode]bool)

    // Helper DFS function to find all nodes at distance K
    var dfs func(node *TreeNode, dist int)
    dfs = func(node *TreeNode, dist int) {
        if node == nil || visited[node] {
            return
        }
        visited[node] = true
        if dist == K {
            result = append(result, node.Val)
            return
        }
        dfs(node.Left, dist+1)
        dfs(node.Right, dist+1)
        dfs(parent[node], dist+1)
    }

    // Start DFS from the target node
    dfs(target, 0)

    return result
}
```

### Complexity Analysis
- **Time Complexity:** O(N), where N is the number of nodes in the tree. We traverse each node once to build parent pointers and once more to perform the DFS.
- **Space Complexity:** O(N), for the parent pointers and visited set.

---

## Approach 2: Breadth-First Search (BFS) with Parent Pointers

### Intuition
Similar to the DFS method, we use BFS to find nodes at distance K. BFS naturally suits problems where we need all nodes at a particular "level" or distance due to its level-order nature.

Steps:
1. Build parent pointers using a DFS traversal.
2. Use BFS starting from the target node. Count the levels until we reach the K-th level, at which point all nodes at this level will be at distance K.

### Code
```go
func distanceK(root, target *TreeNode, K int) []int {
    parent := make(map[*TreeNode]*TreeNode)

    // Build parent pointers
    var buildParentPointers func(node *TreeNode, par *TreeNode)
    buildParentPointers = func(node *TreeNode, par *TreeNode) {
        if node == nil {
            return
        }
        parent[node] = par
        buildParentPointers(node.Left, node)
        buildParentPointers(node.Right, node)
    }

    buildParentPointers(root, nil)

    // BFS queue
    queue := []*TreeNode{target}
    visited := make(map[*TreeNode]bool)
    visited[target] = true
    currentDist := 0

    for len(queue) > 0 {
        if currentDist == K {
            var result []int
            for _, node := range queue {
                result = append(result, node.Val)
            }
            return result
        }
        
        size := len(queue)
        for i := 0; i < size; i++ {
            node := queue[0]
            queue = queue[1:]

            // Check neighbors: left, right, and parent
            if node.Left != nil && !visited[node.Left] {
                visited[node.Left] = true
                queue = append(queue, node.Left)
            }
            if node.Right != nil && !visited[node.Right] {
                visited[node.Right] = true
                queue = append(queue, node.Right)
            }
            if parent[node] != nil && !visited[parent[node]] {
                visited[parent[node]] = true
                queue = append(queue, parent[node])
            }
        }
        currentDist++
    }
    
    return []int{}
}
```

### Complexity Analysis
- **Time Complexity:** O(N), where N is the number of nodes in the tree. We traverse each node to build parent pointers and another traversal for BFS.
- **Space Complexity:** O(N), for parent pointers and BFS queue.

