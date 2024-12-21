# [Leetcode 1110: Delete Nodes And Return Forest](https://leetcode.com/problems/delete-nodes-and-return-forest/)

## Approaches
- [Approach 1: Recursion with Postorder Traversal](#approach-1-recursion-with-postorder-traversal)

---

## Approach 1: Recursion with Postorder Traversal

### Intuition
The problem requires us to delete certain nodes from the binary tree and return a forest (collection of disjoint trees) of trees that result from the split at the deleted nodes. To accomplish this, we can leverage postorder traversal (left, right, root) as it allows us to process child nodes before processing the parent node, which is beneficial when modifying the tree structure. The key steps involve:

1. Recursively traverse the tree using postorder traversal.
2. If a node needs to be deleted:
    - If it has non-null children, treat them as potential new roots and add them to the resulting forest.
3. Return `null` for the node to be deleted, effectively removing the reference to it in the tree.

We use a set to store the values of nodes that need to be deleted for quick lookup.

### Algorithm
1. Convert the `to_delete` list to a set for efficient lookup.
2. Define a recursive function `postorder(node, isRoot)`:
    - If the node is `null`, return `null`.
    - Determine if the current node needs to be deleted using the set.
    - If the current node is a root and it should not be deleted, add it to the result forest.
    - Recursively call `postorder` on the left and right children, with `isRoot` set according to whether the parent node is being deleted.
    - Return `null` if the current node needs to be deleted, otherwise return the node itself.
3. Initiate the traversal with the root node.
4. Return the resulting forest of trees.

### Code
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */

/**
 * @param {TreeNode} root
 * @param {number[]} to_delete
 * @return {TreeNode[]}
 */
var delNodes = function(root, to_delete) {
    const toDeleteSet = new Set(to_delete);
    const forest = [];

    // Postorder traversal helper function
    function postorder(node, isRoot) {
        if (!node) return null;

        // Determine if current node needs to be deleted
        const toDelete = toDeleteSet.has(node.val);
        // If it's a new root (and not deleted), add it to result forest
        if (isRoot && !toDelete) {
            forest.push(node);
        }

        // Recursively process the left and right subtree
        node.left = postorder(node.left, toDelete);
        node.right = postorder(node.right, toDelete);

        // Return null if this node needs to be deleted (disconnect it)
        return toDelete ? null : node;
    }
    
    // Start the recursive process
    postorder(root, true);
    
    return forest;
};
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the number of nodes in the tree. Each node is visited once.
- **Space Complexity:** O(n + m), where n is the recursion stack space in the worst case and m is the size of the set containing nodes to delete.

