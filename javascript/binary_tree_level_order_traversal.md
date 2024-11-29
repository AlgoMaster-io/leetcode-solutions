# 102. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## Approach 1: Breadth-First Search (Using Queue)

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage
function levelOrder(root) {
    const result = [];
    if (root === null) {
        return result; // Return empty result if the tree is empty
    }

    const queue = [];
    queue.push(root);

    while (queue.length !== 0) {
        const levelSize = queue.length;
        const currentLevel = [];

        for (let i = 0; i < levelSize; i++) {
            const currentNode = queue.shift(); // Process the current node
            currentLevel.push(currentNode.val);

            if (currentNode.left !== null) {
                queue.push(currentNode.left); // Add left child to the queue
            }
            if (currentNode.right !== null) {
                queue.push(currentNode.right); // Add right child to the queue
            }
        }

        result.push(currentLevel); // Add the current level to the result
    }

    return result;
}
```

## Approach 2: Depth-First Search (Recursive)

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
function levelOrder(root) {
    const result = [];
    dfs(root, 0, result);
    return result;
}

function dfs(node, level, result) {
    if (node === null) {
        return; // Base case: if the node is null, return
    }

    if (level === result.length) {
        result.push([]); // Create a new level in the result
    }

    result[level].push(node.val); // Add the current node's value to the current level

    dfs(node.left, level + 1, result); // Recurse for the left child
    dfs(node.right, level + 1, result); // Recurse for the right child
}
```

