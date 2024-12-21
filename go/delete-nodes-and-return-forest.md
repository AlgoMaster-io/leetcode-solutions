# [Leetcode 1110: Delete Nodes And Return Forest](https://leetcode.com/problems/delete-nodes-and-return-forest/)

### Approaches:
- [Approach 1: Recursive DFS](#approach-1-recursive-dfs)

## Approach 1: Recursive DFS

Our goal is to process each node in the binary tree and determine whether it needs to be deleted or retained. If a node is deleted, we must ensure that its children (if any) become roots of new trees in the resulting forest.

### Intuition:

To achieve this, we can perform a depth-first search (DFS) on the tree:
1. Traverse the tree using DFS.
2. Whenever we encounter a node that is marked for deletion, split its left and right children into separate trees.
3. If a node is not marked for deletion and is either the root or a child of a deleted node, make sure it is included in the result forest.
4. Keep track of nodes to be deleted efficiently using a hash set for constant time O(1) checks.

### Steps:
- Implement a recursive function that processes each node:
  - Check if the node needs to be deleted.
  - Recursively process the left and right children before processing the current node.
  - If the node is not deleted and:
    - It was a root or a direct child of a deleted node, add it to the result.
  - Return the current node if it should not be deleted, otherwise return null.
  
- Initialize DFS from the tree's root and maintain a list of tree roots in the resulting forest.

### Code:

```go
// TreeNode is the definition for a binary tree node.
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

// Function to process tree deletion according to 'to_delete' list
func delNodes(root *TreeNode, to_delete []int) []*TreeNode {
    // Create a set from to_delete nodes for faster access
    toDeleteSet := make(map[int]bool)
    for _, val := range to_delete {
        toDeleteSet[val] = true
    }
    
    var result []*TreeNode
    
    // Define DFS function to traverse the tree
    var dfs func(node *TreeNode, isRoot bool) *TreeNode
    dfs = func(node *TreeNode, isRoot bool) *TreeNode {
        if node == nil {
            return nil
        }
        
        // Check if current node needs to be deleted
        deleted := toDeleteSet[node.Val]
        
        // If it is a root and not deleted, add to result
        if isRoot && !deleted {
            result = append(result, node)
        }
        
        // Process left and right children
        node.Left = dfs(node.Left, deleted) // if current node is deleted, children become roots
        node.Right = dfs(node.Right, deleted)
        
        // Return nil if the current node is deleted, else return it
        if deleted {
            return nil
        }
        return node
    }
    
    // Start DFS with the root
    dfs(root, true)
    
    return result
}
```

### Time Complexity:

- **O(n)** where `n` is the number of nodes in the tree. Each node is processed once.

### Space Complexity:
- **O(h + d)**, where `h` is the height of the tree (due to recursion stack) and `d` is the number of nodes in `to_delete` (space for the hash set). In the worst case, this results in O(n) space.

