# 199. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

## Approach 1: Breadth-First Search (Using Queue)

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

function rightSideView(root: TreeNode | null): number[] {
    const result: number[] = [];
    if (root === null) {
        return result; // Return empty result if the tree is empty
    }

    const queue: TreeNode[] = [];
    queue.push(root);

    while (queue.length !== 0) {
        const levelSize = queue.length;
        let lastNode: TreeNode | null = null;

        for (let i = 0; i < levelSize; i++) {
            const currentNode = queue.shift()!;
            lastNode = currentNode;

            if (currentNode.left !== null) {
                queue.push(currentNode.left); // Add left child to the queue
            }
            if (currentNode.right !== null) {
                queue.push(currentNode.right); // Add right child to the queue
            }
        }

        result.push(lastNode.val); // Add the last node's value from this level
    }

    return result;
}
```

## Approach 2: Depth-First Search (Recursive)

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

function rightSideView(root: TreeNode | null): number[] {
    const result: number[] = [];
    dfs(root, 0, result);
    return result;
}

function dfs(node: TreeNode | null, depth: number, result: number[]): void {
    if (node === null) {
        return; // Base case: if the node is null, return
    }

    if (depth === result.length) {
        result.push(node.val); // Add the first node of this depth (rightmost node)
    }

    dfs(node.right, depth + 1, result); // Visit the right subtree first
    dfs(node.left, depth + 1, result); // Visit the left subtree next
}
```


