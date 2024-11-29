# 783. [Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

## Approach 1: Inorder Traversal with a List

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the inorder traversal
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = val === undefined ? 0 : val;
        this.left = left === undefined ? null : left;
        this.right = right === undefined ? null : right;
    }
}

function minDiffInBST(root: TreeNode | null): number {
    const values: number[] = [];
    inorder(root, values);

    let minDiff = Number.MAX_VALUE;
    for (let i = 1; i < values.length; i++) {
        minDiff = Math.min(minDiff, values[i] - values[i - 1]);
    }

    return minDiff;
}

function inorder(node: TreeNode | null, values: number[]): void {
    if (node === null) {
        return;
    }

    inorder(node.left, values); // Recurse for the left subtree
    values.push(node.val); // Add the current node value
    inorder(node.right, values); // Recurse for the right subtree
}
```

## Approach 2: Inorder Traversal with Constant Space

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
class Solution {
    private prev: number | null = null;
    private minDiff: number = Number.MAX_VALUE;

    public minDiffInBST(root: TreeNode | null): number {
        this.inorder(root);
        return this.minDiff;
    }

    private inorder(node: TreeNode | null): void {
        if (node === null) {
            return;
        }

        this.inorder(node.left); // Recurse for the left subtree

        if (this.prev !== null) {
            this.minDiff = Math.min(this.minDiff, node.val - this.prev); // Calculate the difference with the previous value
        }
        this.prev = node.val; // Update the previous node value

        this.inorder(node.right); // Recurse for the right subtree
    }
}
```

## Approach 3: Morris Traversal (Space Optimized)

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
function minDiffInBST_Morris(root: TreeNode | null): number {
    let current: TreeNode | null = root;
    let prev: TreeNode | null = null;
    let minDiff: number = Number.MAX_VALUE;

    while (current !== null) {
        if (current.left === null) {
            // Process the current node
            if (prev !== null) {
                minDiff = Math.min(minDiff, current.val - prev.val);
            }
            prev = current;
            current = current.right; // Move to the right subtree
        } else {
            let predecessor: TreeNode | null = current.left;

            // Find the rightmost node in the left subtree
            while (predecessor.right !== null && predecessor.right !== current) {
                predecessor = predecessor.right;
            }

            if (predecessor.right === null) {
                predecessor.right = current; // Create a temporary thread to the root
                current = current.left; // Move to the left subtree
            } else {
                predecessor.right = null; // Remove the temporary thread
                if (prev !== null) {
                    minDiff = Math.min(minDiff, current.val - prev.val);
                }
                prev = current;
                current = current.right; // Move to the right subtree
            }
        }
    }

    return minDiff;
}
```

