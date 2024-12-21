# [Leetcode 100: Same Tree](https://leetcode.com/problems/same-tree/)

## Approaches
1. [Recursive Depth-First Search (DFS)](#recursive-depth-first-search-dfs)
2. [Iterative Breadth-First Search (BFS) Using Queue](#iterative-breadth-first-search-bfs-using-queue)

### Recursive Depth-First Search (DFS)

#### Intuition
The problem can be naturally solved by using a recursive approach. This involves checking if the current nodes in both trees are equal in terms of value and structure, and then recursively checking their left and right children. If both these conditions hold true for the entire tree, then the two trees are the same.

#### Approach
1. **Base Case**: If both nodes are `nil`, return `true`.
2. If one of the nodes is `nil` and the other is not, return `false`.
3. Check if the current nodes of both trees have the same value.
4. Recursively check both left and right subtrees.

```go
// Definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func isSameTree(p *TreeNode, q *TreeNode) bool {
    if p == nil && q == nil {
        return true
    }
    if p == nil || q == nil {
        return false
    }
    if p.Val != q.Val {
        return false
    }
    return isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}
```

#### Complexity Analysis
- **Time Complexity**: O(N), where N is the minimum number of nodes in both trees. This is because in the worst case scenario, each node in both trees will be visited exactly once.
- **Space Complexity**: O(H), where H is the depth of the tree, accounting for the call stack in the recursion.

### Iterative Breadth-First Search (BFS) Using Queue

#### Intuition
Instead of using recursion, we can also use a queue to iteratively solve this problem. This involves using a BFS approach to check if both trees are the same.

#### Approach
1. Initialize a queue and add a pair of root nodes from both trees.
2. For each node pair in the queue:
   - Dequeue a pair of nodes.
   - If both are `nil`, continue.
   - If one of them is `nil`, return `false`.
   - If their values are not equal, return `false`.
   - Enqueue left children of both nodes.
   - Enqueue right children of both nodes.
3. If the queue is exhausted without issue, return `true`.

```go
type Pair struct {
    Node1 *TreeNode
    Node2 *TreeNode
}

func isSameTree(p *TreeNode, q *TreeNode) bool {
    queue := []Pair{{p, q}}
    
    for len(queue) > 0 {
        pair := queue[0]
        queue = queue[1:]
        
        node1, node2 := pair.Node1, pair.Node2
        
        if node1 == nil && node2 == nil {
            continue
        }
        
        if node1 == nil || node2 == nil || node1.Val != node2.Val {
            return false
        }
        
        queue = append(queue, Pair{node1.Left, node2.Left})
        queue = append(queue, Pair{node1.Right, node2.Right})
    }
    
    return true
}
```

#### Complexity Analysis
- **Time Complexity**: O(N), similar to the recursive approach as we need to visit each node at least once.
- **Space Complexity**: O(W), where W is the maximum width of the tree, corresponding to the queue size at any level of the tree.

