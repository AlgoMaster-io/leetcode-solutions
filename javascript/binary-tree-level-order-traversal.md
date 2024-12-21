# [Leetcode 102: Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## Table of Contents
1. [Approach 1: Breadth-First Search using Queue (Level Order Traversal)](#approach-1-breadth-first-search-using-queue-level-order-traversal)
2. [Approach 2: Depth-First Search with Level Tracking (Recursive)](#approach-2-depth-first-search-with-level-tracking-recursive)

## Approach 1: Breadth-First Search using Queue (Level Order Traversal)

### Intuition
The most intuitive way to solve this problem is by using a Breadth-First Search (BFS). The idea is to traverse the tree one level at a time, which naturally aligns with using a queue data structure. The queue helps in tracking the nodes to visit at each level.

### Steps
1. Initialize a queue and enqueue the root node.
2. While the queue is not empty:
   - Determine the number of nodes at the current level.
   - Iterate through all nodes at this level and:
     - Dequeue a node, add its value to the current level's list.
     - Enqueue its left and right children (if they exist) for the next level.
3. Add each level's list of nodes to the results list.

### Code
```javascript
function levelOrder(root) {
    if (!root) return []; // If the tree is empty, return an empty array

    const result = [];
    const queue = [root]; // Initialize the queue with the root node

    while (queue.length > 0) {
        const levelSize = queue.length; // Number of nodes at the current level
        const currentLevel = [];

        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift(); // Dequeue the front node
            currentLevel.push(node.val); // Add the node's value to the current level

            // Enqueue the node's children for the next level
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
        
        result.push(currentLevel); // Add this level to the result
    }

    return result;
}
```
### Time Complexity
- **O(n)**, where `n` is the number of nodes in the tree. Each node is processed exactly once.

### Space Complexity
- **O(n)**, due to the storage of node values in the queue and result list.

## Approach 2: Depth-First Search with Level Tracking (Recursive)

### Intuition
An alternative solution uses Depth-First Search (DFS) with recursion to traverse the tree while keeping track of the current depth or level. Each recursive call processes one node and adds its value to a list corresponding to the node's depth.

### Steps
1. Define a recursive helper function that:
   - Adds the current node's value to the result list at the index corresponding to the level.
   - Recursively visits its left child and then the right child, increasing the level as you go deeper.
2. Manage the result such that each level corresponds to an index in the result list.

### Code
```javascript
function levelOrderDFS(root) {
    const result = [];

    function dfs(node, level) {
        if (!node) return; // Base case: if the node is null, return

        // If we're visiting a new level for the first time, create a new array for it
        if (!result[level]) {
            result[level] = [];
        }

        result[level].push(node.val); // Add the node's value to its respective level

        // Recursively visit left and right children, moving to the next level
        dfs(node.left, level + 1);
        dfs(node.right, level + 1);
    }

    dfs(root, 0); // Start the DFS traversal from the root at level 0
    return result;
}
```
### Time Complexity
- **O(n)**, where `n` is the number of nodes. Every node is visited exactly once.

### Space Complexity
- **O(n)**, due to the call stack used by recursion and the storage of nodes' values.

