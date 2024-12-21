# [Leetcode 662: Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/)

## Table of Contents
- [Approach 1: Level Order Traversal with Indices (Basic)](#approach-1-level-order-traversal-with-indices-basic)
- [Approach 2: Optimized Level Order Traversal with Normalization (Optimal)](#approach-2-optimized-level-order-traversal-with-normalization-optimal)

## Approach 1: Level Order Traversal with Indices (Basic)

### Intuition
The basic idea is to perform a level-order traversal using a queue while maintaining indices that represent each node's position as if the tree was a complete binary tree. This helps in determining the width of the tree at each level because the width is the difference between the index of the last and first node at that level plus one.

### Steps
1. Initialize a queue to perform level-order traversal. Each element in the queue contains a reference to the node and its index.
2. For each level in the tree, store the indices of the first and last node.
3. Compute the width of the level and update the maximum width.
4. Store children nodes in the queue with their corresponding indices.
5. Return the maximum width found.

### Code
```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.left = (left===undefined ? null : left);
        this.right = (right===undefined ? null : right);
    }
}

function widthOfBinaryTree(root: TreeNode | null): number {
    if (!root) return 0;
    let maxWidth = 0;
    const queue: [TreeNode, number][] = [[root, 0]]; // Queue with node and index

    while (queue.length > 0) {
        const levelLength = queue.length;
        const firstIndex = queue[0][1]; // Index of the first node in this level
        let lastIndex = firstIndex;
        
        for (let i = 0; i < levelLength; i++) {
            const [node, index] = queue.shift()!;
            lastIndex = index;
            // Enqueue left and right children with calculated indices
            if (node.left) {
                queue.push([node.left, 2 * index]);
            }
            if (node.right) {
                queue.push([node.right, 2 * index + 1]);
            }
        }
        // Compute the width of the level
        maxWidth = Math.max(maxWidth, lastIndex - firstIndex + 1);
    }
    
    return maxWidth;
}
```

### Complexity
- **Time Complexity:** \(O(n)\) because each node is processed once.
- **Space Complexity:** \(O(n)\) to store node indices in the queue.

## Approach 2: Optimized Level Order Traversal with Normalization (Optimal)

### Intuition
To avoid large indices which can cause integer overflow in some languages, we normalize the index by subtracting the smallest index at each level. This keeps the indices small and manageable while preserving the relative order of nodes, allowing us to compute the width accurately.

### Steps
1. Initialize the queue as before with nodes and their indices, but normalize indices at each level by subtracting the index of the first node at that level.
2. Compute the width for the current level using normalized indices.
3. Store children nodes in the queue with normalized indices to ensure accurate calculations.
4. Return the maximum width computed across all levels.

### Code
```typescript
function widthOfBinaryTree(root: TreeNode | null): number {
    if (!root) return 0;
    let maxWidth = 0;
    const queue: [TreeNode, number][] = [[root, 0]]; // Queue with node and normalized index

    while (queue.length > 0) {
        const levelLength = queue.length;
        const firstIndex = queue[0][1]; // Index of the first node in this level
        let lastIndex = firstIndex;

        for (let i = 0; i < levelLength; i++) {
            const [node, index] = queue.shift()!;
            const normalizedIndex = index - firstIndex; // Normalize to avoid large numbers
            lastIndex = normalizedIndex;
            // Enqueue children with normalized indices
            if (node.left) {
                queue.push([node.left, 2 * normalizedIndex]);
            }
            if (node.right) {
                queue.push([node.right, 2 * normalizedIndex + 1]);
            }
        }
        // Calculate width of current level
        maxWidth = Math.max(maxWidth, lastIndex + 1);
    }
    
    return maxWidth;
}
```

### Complexity
- **Time Complexity:** \(O(n)\) as each node is processed exactly once.
- **Space Complexity:** \(O(n)\) for the queue storage.

