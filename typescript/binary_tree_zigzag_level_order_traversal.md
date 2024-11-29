# 103. [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Approach 1: Breadth-First Search with Direction Toggle

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage

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

function zigzagLevelOrder(root: TreeNode | null): number[][] {
    const result: number[][] = [];
    if (root === null) {
        return result; // Return empty result if the tree is empty
    }

    const queue: TreeNode[] = [];
    queue.push(root);
    let leftToRight = true;

    while (queue.length > 0) {
        const levelSize = queue.length;
        const currentLevel: number[] = [];

        for (let i = 0; i < levelSize; i++) {
            const currentNode = queue.shift()!; // Process the current node

            if (leftToRight) {
                currentLevel.push(currentNode.val); // Add at the end
            } else {
                currentLevel.unshift(currentNode.val); // Add at the beginning
            }

            if (currentNode.left !== null) {
                queue.push(currentNode.left); // Add left child to the queue
            }
            if (currentNode.right !== null) {
                queue.push(currentNode.right); // Add right child to the queue
            }
        }

        result.push(currentLevel); // Add the current level to the result
        leftToRight = !leftToRight; // Toggle the direction for the next level
    }

    return result;
}
```

## Approach 2: Depth-First Search (Recursive with Level Tracking)

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

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

function zigzagLevelOrder(root: TreeNode | null): number[][] {
    const result: number[][] = [];
    dfs(root, 0, result);
    return result;
}

function dfs(node: TreeNode | null, level: number, result: number[][]): void {
    if (node === null) {
        return; // Base case: if the node is null, return
    }

    if (level === result.length) {
        result.push([]); // Create a new level in the result
    }

    if (level % 2 === 0) {
        result[level].push(node.val); // Add normally for even levels
    } else {
        result[level].unshift(node.val); // Add to the front for odd levels
    }

    dfs(node.left, level + 1, result); // Recurse for the left child
    dfs(node.right, level + 1, result); // Recurse for the right child
}
```

