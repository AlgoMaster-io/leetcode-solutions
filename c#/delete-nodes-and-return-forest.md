# [Leetcode 1110: Delete Nodes And Return Forest](https://leetcode.com/problems/delete-nodes-and-return-forest/)

## Approaches:
1. [Recursive Depth-First Search (DFS)](#recursive-depth-first-search-dfs)
2. [DFS with Parent Tracking](#dfs-with-parent-tracking)

### Recursive Depth-First Search (DFS)

#### Intuition:
The problem involves deleting certain nodes from a binary tree and returning the resulting forest, which is composed of tree roots that are not deleted and are not attached to any deleted nodes.

- A recursive DFS approach is suitable here, allowing us to navigate the tree and determine which nodes should be retained as part of the forest.
- We maintain a set of nodes to delete and traverse the tree. If a node is found in the set, it's marked for deletion, meaning its children could potentially be part of the forest.
- As we process each node, if it's marked for deletion, we add its non-null children to the result forest.

#### Steps:
1. Convert the list of nodes to delete into a set for quick lookup.
2. Perform a DFS traversal of the tree. If the current node should not be deleted (i.e., it's not in the set), return it as part of the tree.
3. If a node is marked for deletion, ensure its children have been processed and potentially added to the result.
4. Use a helper function to process each subtree, recursively deleting nodes and adding root nodes to the forest as necessary.

#### Code:

```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public IList<TreeNode> DelNodes(TreeNode root, int[] to_delete) {
        HashSet<int> toDeleteSet = new HashSet<int>(to_delete);
        List<TreeNode> forest = new List<TreeNode>();
        
        root = DeleteNodes(root, toDeleteSet, forest);
        if (root != null)
            forest.Add(root);
        
        return forest;
    }
    
    private TreeNode DeleteNodes(TreeNode node, HashSet<int> toDeleteSet, List<TreeNode> forest) {
        if (node == null) return null;

        // Recurse on left and right children
        node.left = DeleteNodes(node.left, toDeleteSet, forest);
        node.right = DeleteNodes(node.right, toDeleteSet, forest);
        
        // If the current node needs to be deleted
        if (toDeleteSet.Contains(node.val)) {
            // If there is a left child, add it to the forest
            if (node.left != null) forest.Add(node.left);
            // If there is a right child, add it to the forest
            if (node.right != null) forest.Add(node.right);
            // Return null to remove this node
            return null;
        }
        
        // If not deleted, return this node
        return node;
    }
}
```

#### Complexity:
- **Time Complexity**: O(N), where N is the number of nodes in the tree. Each node is visited once.
- **Space Complexity**: O(N + D), where D is the number of nodes to delete, due to the additional data structures used.

### DFS with Parent Tracking

This approach would be similar in terms of complexity but would explicitly track parent nodes and their changes, which isn't generally required for this problem as recursion inherently manages the tree structure. Therefore, the recursive DFS method suffices and efficiently handles the problem requirements. 

Each Recursive DFS implementation above should be used also considering edge cases like having one node only, or all nodes being deleted.

