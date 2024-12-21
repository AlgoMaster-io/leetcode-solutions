# [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Iterative Approach with Parent Pointers](#iterative-approach-with-parent-pointers)
- [Iterative Approach with Stack](#iterative-approach-with-stack)

## Recursive Approach

### Intuition:
The idea is to traverse the tree in a depth-first manner. We need to find the lowest common ancestor of two nodes `p` and `q`. The lowest common ancestor problem in a binary tree can be approached using a recursive post-order traversal. In this manner, we check if the current node is one of the nodes we are looking for or if either of the nodes can be found in the subtrees. If both nodes can be found by traversing different subtrees of a node, then that node is the LCA.

### Algorithm Steps:
1. Traverse the tree starting from the root node.
2. If you reach the end of the branch, return `nil`.
3. If the current node being checked is either `p` or `q`, return that node up the recursive chain.
4. Recursively check the left and right subtrees for `p` and `q`.
5. If both left and right recursive calls return non-null values, the current node is the lowest common ancestor, thus return it.
6. If only one side is non-null, return that side.

### Time and Space Complexity:
- Time Complexity: O(N), where N is the number of nodes in the tree. In the worst case, we might visit all the nodes.
- Space Complexity: O(H), where H is the height of the tree, representing the recursive call stack.

### Code:
```go
/**
 * Definition for TreeNode.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    // If current node is nil, there is nothing to consider, return nil
    if root == nil {
        return nil
    }
    
    // If the current node is either p or q, return the current node
    if root == p || root == q {
        return root
    }

    // Search for p and q in the left subtree
    left := lowestCommonAncestor(root.Left, p, q)
    
    // Search for p and q in the right subtree
    right := lowestCommonAncestor(root.Right, p, q)

    // If both left and right returned non-nil, it means both p and q are found in different branches
    // Hence, the current root is their lowest common ancestor
    if left != nil && right != nil {
        return root
    }
    
    // If either left or right children returned a node, means both p and q are located in one subtree
    // Return that non-nil node up the recursive call stack
    if left != nil {
        return left
    }
    
    return right
}
```

## Iterative Approach with Parent Pointers

### Intuition:
In this solution, we iterate over the tree in order to build a parent pointer (a map), which will map each child to its parent. Then starting from the nodes `p` and `q`, we track their ancestors by moving upwards towards the root using the parent pointer.

### Algorithm Steps:
1. Use a stack to do pre-order traversal and maintain a parent pointer (map) for each node.
2. Use the parent pointers to walk through the ancestors of node `p` and store them in a set.
3. Iterate over the ancestors of node `q` and return the first one that appears in `p`'s ancestor set.

### Time and Space Complexity:
- Time Complexity: O(N), because we iterate over the tree once.
- Space Complexity: O(N), due to the auxiliary data structures.

### Code:
```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    // Create a map to store parent pointers of each node
    parent := make(map[*TreeNode]*TreeNode)
    // Stack to perform the DFS
    stack := []*TreeNode{root}
    
    // Populate the parent map using the stack for DFS
    parent[root] = nil
    for len(parent) < 2 {
        node := stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        
        // If left child is present, add to stack and map its parent
        if node.Left != nil {
            parent[node.Left] = node
            stack = append(stack, node.Left)
        }
        
        // If right child is present, add to stack and map its parent
        if node.Right != nil {
            parent[node.Right] = node
            stack = append(stack, node.Right)
        }
    }
    
    // Ancestry set for node p
    ancestors := make(map[*TreeNode]bool)
    for p != nil {
        ancestors[p] = true
        p = parent[p]
    }
    
    // Find the first ancestor of q which is also an ancestor of p
    for !ancestors[q] {
        q = parent[q]
    }
    
    return q
}
```

## Iterative Approach with Stack

### Intuition:
In this more efficient approach, we aim to use a single traversal to find both the nodes and determine their lowest common ancestor using a stack without explicit parent pointers.

### Algorithm Steps:
1. Initiate a stack with the root and a visited list.
2. As we pop from the stack, simulate the recursion stack by handling already visited nodes and processing nodes similar to recursive depth-first traversal.
3. Continue finding nodes `p` and `q` and clearing nodes from the stack until both are located within the stack space. 

### Time and Space Complexity:
- Time Complexity: O(N)
- Space Complexity: O(N)

### Code:
```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    stack := []*TreeNode{root}
    parent := map[*TreeNode]*TreeNode{root: nil}
    
    // Standard DFS to fill parent pointers
    for !(parent[p] != nil && parent[q] != nil) {
        node := stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        
        if node.Left != nil {
            parent[node.Left] = node
            stack = append(stack, node.Left)
        }
        if node.Right != nil {
            parent[node.Right] = node
            stack = append(stack, node.Right)
        }
    }

    ancestors := map[*TreeNode]bool{}
    
    // Store ancestors of p
    for p != nil {
        ancestors[p] = true
        p = parent[p]
    }
    
    // Determine the LCA for q
    for !ancestors[q] {
        q = parent[q]
    }
    
    return q
}
```

These approaches cover the basic to more advanced methods of solving the Lowest Common Ancestor problem in a binary tree using both recursion and iterative techniques. Each approach illustrates how a binary tree can be traversed and processed in different ways to achieve the desired result.

