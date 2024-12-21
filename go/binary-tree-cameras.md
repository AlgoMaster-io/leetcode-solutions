# [Leetcode 968: Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approaches
- [Approach 1: Recursive DFS with Greedy Strategy](#approach-1)
- [Approach 2: Bottom-Up Dynamic Programming](#approach-2)

---

## Approach 1: Recursive DFS with Greedy Strategy

### Intuition
The essence of this problem is to place cameras in such a way that each node is either monitored directly by a camera, or it is monitored by a camera situated in any of its direct children or parent. We can use a greedy DFS approach to decide camera placement: starting from the leaves upward, determine if placing a camera increases the number of monitored nodes efficiently. 

We can categorize nodes into three states:
1. `0` - The node is not covered.
2. `1` - The node is covered but has no camera.
3. `2` - The node has a camera.

Using these classifications, an optimal strategy is to check if both children of a node are covered, then decide camera placement.

### Detailed Steps
1. Perform a DFS on the tree.
2. If both children of a node are covered but have no camera, place a camera at this node.
3. If either child of a node does not have a camera and is not covered, the parent needs to have a camera to cover such children. 

### Code

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val   int
 *     Left  *TreeNode
 *     Right *TreeNode
 * }
 */
func minCameraCover(root *TreeNode) int {
    cameras := 0
    
    var dfs func(node *TreeNode) int
    dfs = func(node *TreeNode) int {
        if node == nil {
            return 1  // A null node is considered covered
        }
        
        left := dfs(node.Left)
        right := dfs(node.Right)
        
        if left == 0 || right == 0 {
            cameras++  // If any child is not covered, place a camera at the current node
            return 2   // Current node now has a camera
        }
        
        if left == 2 || right == 2 {
            return 1  // If any child has a camera, current node is covered without a camera
        }
        
        return 0  // Otherwise, current node is not covered
    }
    
    if dfs(root) == 0 {  // If root node is not covered, place a camera
        cameras++
    }
    
    return cameras
}
```

### Complexity Analysis
- **Time Complexity**: O(N) where N is the number of nodes in the tree, as we traverse each node exactly once.
- **Space Complexity**: O(H) where H is the height of the tree. This space is used by the system stack during the recursive calls in DFS.

---

## Approach 2: Bottom-Up Dynamic Programming

### Intuition
The bottom-up dynamic programming approach refines the recursive DFS method by explicitly using dynamic programming states to minimize redundant calculations. Here, we explicitly track three states:

1. `dp[0]` - Minimum cameras to cover the subtree rooted at the current node when the node is not covered.
2. `dp[1]` - Minimum cameras to cover the subtree when the current node is covered and does not contain a camera.
3. `dp[2]` - Minimum cameras to cover the subtree when the current node has a camera.

This is similar to our greedy approach but more systematically considers potential placements, effectively allowing dynamic programming to cover the tree optimally.

### Code

```go
func minCameraCover(root *TreeNode) int {
    var dfs func(node *TreeNode) [3]int
    dfs = func(node *TreeNode) [3]int {
        if node == nil {
            return [3]int{0, 0, 9999} // Use large number to prevent placing cameras on null nodes
        }
        
        left := dfs(node.Left)
        right := dfs(node.Right)
        
        dp := [3]int{0, 0, 0}
        dp[0] = left[1] + right[1]
        dp[1] = min(left[2]+right[1], right[2]+left[1])
        dp[2] = 1 + min(left[0], min(left[1], left[2])) + min(right[0], min(right[1], right[2]))
        
        return dp
    }
    
    result := dfs(root)
    return min(result[1], result[2])
}

func min(x, y int) int {
    if x < y {
        return x
    }
    return y
}
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes. Each node is processed once.
- **Space Complexity**: O(H) where H is the height of the tree, representing the stack space used for recursion.

