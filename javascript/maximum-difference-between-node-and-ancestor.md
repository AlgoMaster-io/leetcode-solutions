# [Leetcode 1026: Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/)

## Approaches
1. [Recursive DFS with Tracking Min and Max](#approach-1)
2. [Optimized DFS with In-place Node Processing](#approach-2)

---

## Approach 1: Recursive DFS with Tracking Min and Max

### Intuition
In this approach, we use a Depth First Search (DFS) to traverse the tree. As we traverse, we maintain two values: the minimum and maximum values seen so far from the root to the current node. For each node, we compute the difference between the node's value and the current min and max, and keep track of the maximum difference found. This approach is straightforward and leverages recursion to handle the tree traversal.

### Detailed Steps
1. Start from the root node, initiating min and max as the value of the root.
2. For each node, calculate the potential maximum difference using the current min and max.
3. Update the current min and max values.
4. Recursively traverse the left and right subtree.
5. Return the maximum difference found.

### Code
```javascript
var maxAncestorDiff = function(root) {
    // Helper function for DFS traversal
    const dfs = (node, currentMin, currentMax) => {
        if (!node) return currentMax - currentMin;

        // Update current min and max with the current node's value
        currentMin = Math.min(currentMin, node.val);
        currentMax = Math.max(currentMax, node.val);

        // Recurse on left and right children
        let leftDiff = dfs(node.left, currentMin, currentMax);
        let rightDiff = dfs(node.right, currentMin, currentMax);

        // Return the maximum difference found
        return Math.max(leftDiff, rightDiff);
    };

    // Start DFS with the root, initializing min and max as root's value
    return dfs(root, root.val, root.val);
};
```

### Time Complexity
- **Time**: O(N), where N is the number of nodes in the tree, since we visit each node exactly once.
- **Space**: O(H), where H is the height of the tree, due to the recursion stack.

## Approach 2: Optimized DFS with In-place Node Processing

### Intuition
This approach is a refinement of the first. Here, rather than computing the maximum difference on visiting leaf nodes, we aim to integrate the calculation as we traverse the nodes. This minor adjustment can simplify the recursion further by minimizing variable overwrites, making it slightly simpler or potentially faster due to fewer operations regarding min/max.

### Detailed Steps
1. Use DFS starting from the root node.
2. As with Approach 1, record current min and max values.
3. Directly compute the max difference as we explore each node, eliminating additional computations post DFS call.

### Code
```javascript
var maxAncestorDiff = function(root) {
    const dfs = (node, currentMin, currentMax) => {
        if (!node) return;

        // Calculate the current potential differences
        const difference = Math.max(Math.abs(node.val - currentMin), Math.abs(node.val - currentMax));

        // Update minimum and maximum from the root to this node
        currentMin = Math.min(currentMin, node.val);
        currentMax = Math.max(currentMax, node.val);

        // Compute differences for the left and right children
        let leftDiff = node.left ? dfs(node.left, currentMin, currentMax) : 0;
        let rightDiff = node.right ? dfs(node.right, currentMin, currentMax) : 0;
        
        // Return the maximum difference encountered
        return Math.max(difference, leftDiff, rightDiff);
    };

    // Initiate DFS with the root node
    return dfs(root, root.val, root.val);
};
```

### Time Complexity
- **Time**: O(N), same as the first approach because each node is visited once.
- **Space**: O(H), with recursion stack space corresponding to the height of the tree.

Both approaches present efficient DFS solutions, each focusing on careful tracking of ancestor values as part of the recursive state, ensuring that we capture the maximum value difference efficiently.

