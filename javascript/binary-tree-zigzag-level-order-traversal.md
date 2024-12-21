# [Leetcode 103: Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Table of Contents
- [Approach 1: Iterative Breadth-First Search (BFS) using Two Stacks](#approach-1)
- [Approach 2: Iterative Breadth-First Search (BFS) using Deque](#approach-2)

## Approach 1: Iterative Breadth-First Search (BFS) using Two Stacks

### Intuition
To perform a zigzag level order traversal, we need to alternate between appending nodes from left-to-right and right-to-left at each level. A convenient way to achieve this is by using two stacks. One stack can handle the nodes at the current level while pushing its children in the desired order to the other stack for the next level.

### Steps
1. Use two stacks to represent the current level and the next level.
2. For the current level, depending on the order (left-to-right or right-to-left):
   - Use stack operations to access nodes and their children for traversal in the desired order.
3. Prepare for the next level by swapping stacks and reversing the working order.

### Code
```javascript
var zigzagLevelOrder = function(root) {
    if (!root) return [];

    const result = [];
    let currentStack = [root];
    let nextStack = [];
    let leftToRight = true;

    while (currentStack.length > 0) {
        const level = [];
        while (currentStack.length > 0) {
            const node = currentStack.pop();
            level.push(node.val);

            if (leftToRight) {
                if (node.left) nextStack.push(node.left);
                if (node.right) nextStack.push(node.right);
            } else {
                if (node.right) nextStack.push(node.right);
                if (node.left) nextStack.push(node.left);
            }
        }
        result.push(level);
        // Swap the stacks
        [currentStack, nextStack] = [nextStack, currentStack];
        // Toggle the direction
        leftToRight = !leftToRight;
    }

    return result;
};
```

### Time and Space Complexity
- **Time Complexity**: `O(n)`, where `n` is the number of nodes in the tree. Each node is processed exactly once.
- **Space Complexity**: `O(n)` for the stacks used to store nodes in each level.

## Approach 2: Iterative Breadth-First Search (BFS) using Deque

### Intuition
An alternate efficient method can use a double-ended queue (deque) instead of two stacks. A deque allows appending and popping operations from both ends, thereby offering a more natural representation for alternating the addition order of nodes per level.

### Steps
1. Use a queue to perform BFS. Begin with the root node.
2. For each level, traverse nodes from the queue:
   - Append node values into a temporary level array considering the current direction.
   - Queue children of the current node for the next level.
3. Flip direction after processing each level.

### Code
```javascript
var zigzagLevelOrder = function(root) {
    if (!root) return [];

    const result = [];
    const queue = [root];
    let leftToRight = true;

    while (queue.length > 0) {
        const levelSize = queue.length;
        const level = new Array(levelSize);

        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift();
            // Find position to fill node's value
            const index = leftToRight ? i : levelSize - 1 - i;
            level[index] = node.val;

            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
        result.push(level);
        leftToRight = !leftToRight;
    }

    return result;
};
```

### Time and Space Complexity
- **Time Complexity**: `O(n)`, with `n` being the total number of nodes, as we visit each node exactly once.
- **Space Complexity**: `O(n)` for storing levels in the queue and results.

