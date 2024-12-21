# [Leetcode 1110: Delete Nodes And Return Forest](https://leetcode.com/problems/delete-nodes-and-return-forest/)

## Approaches
- [Solution 1: Recursive DFS Approach](#solution-1)
- [Solution 2: Optimized Recursive DFS with Set](#solution-2)

---

## Solution 1: Recursive DFS Approach

### Intuition
The problem requires us to return a forest of trees by deleting certain nodes from a binary tree. We can tackle this problem using a recursive approach. The primary idea is to traverse the tree using DFS (Depth First Search) and make decisions at each node whether it is to be deleted or retained.

1. For each node, recursively decide for its left and right children whether they need to be deleted.
2. If a node is to be deleted, its children are added to the result list of roots if they are non-null.
3. If a node is not deleted, and it is a root (i.e., the start of recursion), add it to the result list of roots.

### Detailed Steps
- Start from the root of the tree.
- Use recursion to traverse the tree.
- At each recursive call, check if the current node is in the delete list.
- If it is, add its children to the forest if they exist.
- If not, keep traversing, possibly marking this node to remain as part of an existing tree.

### Java Implementation

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Solution {
    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
        List<TreeNode> forest = new ArrayList<>();
        Set<Integer> toDeleteSet = new HashSet<>();
        for (int value : to_delete) {
            toDeleteSet.add(value);
        }
        deleteNodes(root, toDeleteSet, forest, true);
        return forest;
    }

    private TreeNode deleteNodes(TreeNode node, Set<Integer> toDeleteSet, List<TreeNode> forest, boolean isRoot) {
        if (node == null) {
            return null;
        }
        
        boolean toDelete = toDeleteSet.contains(node.val);
        
        if (isRoot && !toDelete) {
            // If it's a root node and not deleted, add to forest.
            forest.add(node);
        }
        
        // Recursively check and delete nodes for its children
        node.left = deleteNodes(node.left, toDeleteSet, forest, toDelete);
        node.right = deleteNodes(node.right, toDeleteSet, forest, toDelete);
        
        // Return null if current node is to be deleted, else return the node itself
        return toDelete ? null : node;
    }
}
```

### Complexity
- **Time Complexity**: \(O(n)\), where \(n\) is the number of nodes in the tree. Each node is processed once.
- **Space Complexity**: \(O(h + d)\), where \(h\) is the height of the tree and \(d\) is the number of nodes to delete (to store in HashSet).

---

## Solution 2: Optimized Recursive DFS with Set

### Intuition
This approach enhances the first solution by using a HashSet to store nodes that need to be deleted, making the check for deletions \(O(1)\) on average. This makes it slightly more optimal for larger sets of nodes to be deleted.

### Steps and Implementation
Same steps as above, but the check for deletion is turned into an \(O(1)\) operation by using a Set data structure.

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Solution {
    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
        List<TreeNode> forest = new ArrayList<>();
        Set<Integer> deleteSet = new HashSet<>();
        
        // Populate the HashSet with nodes to delete for an O(1) lookup.
        for (int val : to_delete) {
            deleteSet.add(val);
        }
        
        // Helper function to perform DFS and manage the forest list.
        helper(root, true, deleteSet, forest);
        
        return forest;
    }
    
    private TreeNode helper(TreeNode node, boolean isRoot, Set<Integer> deleteSet, List<TreeNode> forest) {
        if (node == null) {
            return null;
        }
        
        // Determine if the current node needs to be deleted.
        boolean deleted = deleteSet.contains(node.val);
        
        if (isRoot && !deleted) {
            forest.add(node);
        }
        
        // Recursively process child nodes, determining new roots based on 'deleted' status.
        node.left = helper(node.left, deleted, deleteSet, forest);
        node.right = helper(node.right, deleted, deleteSet, forest);

        // Return null if node is deleted, otherwise return the current node.
        return deleted ? null : node;
    }
}
```

### Complexity
- **Time Complexity**: \(O(n)\), as each node is visited only once.
- **Space Complexity**: \(O(h + d)\).
  - The \(O(h)\) is for the recursion stack (maximum is the height of the binary tree).
  - The \(O(d)\) is for the set containing nodes to be deleted (if using mutable data structures for large delete lists).

