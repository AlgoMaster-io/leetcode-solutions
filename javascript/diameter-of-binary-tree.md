# [Leetcode 543: Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

## Approaches
- [Approach 1: Recursive Depth-First Search (DFS)](#approach-1-recursive-depth-first-search-dfs)
- [Approach 2: Optimized DFS with single pass](#approach-2-optimized-dfs-with-single-pass)

## Approach 1: Recursive Depth-First Search (DFS)

### Intuition
The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root. To solve this problem, our aim is to find the greatest number of nodes on any path in the tree. In a binary tree, this path is often represented by the sum of the heights of left and right subtrees for any node.

A straightforward approach to tackling this problem is to calculate the depth of each subtree recursively and compute the possible diameter at each node. Keep track of the maximum diameter found during the recursion.

### Steps:
1. Define a helper recursive function `depth` which computes the depth of a subtree rooted at a given node.
2. For each node, compute the left depth and right depth.
3. Update the max diameter with `left depth + right depth`.
4. Return the maximum depth of the node's subtrees (i.e., `1 + max(left depth, right depth)`).

### Code
```javascript
function diameterOfBinaryTree(root) {
    if (!root) return 0;
    
    let maxDiameter = 0;

    function depth(node) {
        if (!node) return 0;

        // Recursively find the depth of the left and right subtrees
        let leftDepth = depth(node.left);
        let rightDepth = depth(node.right);

        // Update the max diameter found so far
        maxDiameter = Math.max(maxDiameter, leftDepth + rightDepth);

        // Return the depth of the subtree rooted at the current node
        return 1 + Math.max(leftDepth, rightDepth);
    }

    // Call the depth function starting from the root
    depth(root);

    return maxDiameter;
}
```

### Time Complexity
- **Time Complexity**: O(N), where N is the number of nodes in the tree. We visit each node once.
- **Space Complexity**: O(H), where H is the height of the tree. This space is used implicitly by the system stack during the recursion.

## Approach 2: Optimized DFS with single pass

### Intuition
The approach remains fundamentally the same as before, but we optimize by making sure that we're computing the diameter at the same time that we're calculating the depths, rather than first calculating depths then using those depths to calculate diameters.

The recursive function now returns both the depth and updates the maximum diameter found so far.

### Steps:
1. Utilize a single recursive function to handle both depth calculation and diameter updating.
2. In a single traversal, compute and return the depth while simultaneously updating the maximum diameter.

### Code
```javascript
function diameterOfBinaryTree(root) {
    let maxDiameter = 0;

    function dfs(node) {
        if (!node) return 0;

        // Calculate depth of left subtree
        let left = dfs(node.left);
        // Calculate depth of right subtree
        let right = dfs(node.right);

        // Calculate the diameter passing through this node
        maxDiameter = Math.max(maxDiameter, left + right);

        // Return the depth of this node - max depth of either subtree + itself
        return 1 + Math.max(left, right);
    }

    dfs(root);

    return maxDiameter;
}
```

### Time Complexity
- **Time Complexity**: O(N), where N is the number of nodes since each node is visited once.
- **Space Complexity**: O(H), which is the height of the tree, as memory is consumed by the stack frames during the depth-first search recursion.

