# 144. [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approach 1: Recursive Preorder Traversal

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

function preorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    const preorder = (node: TreeNode | null) => {
        if (node === null) return;

        result.push(node.val); // Visit the root
        preorder(node.left);  // Recurse for the left subtree
        preorder(node.right); // Recurse for the right subtree
    };

    preorder(root);
    return result;
}
```

## Approach 2: Iterative Preorder Traversal Using Stack

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack

function preorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    if (root === null) return result; // Return empty result if the tree is empty

    const stack: TreeNode[] = [root];

    while (stack.length) {
        const currentNode = stack.pop()!;
        result.push(currentNode.val); // Visit the root

        // Push right child first so that left child is processed first
        if (currentNode.right !== null) {
            stack.push(currentNode.right);
        }
        if (currentNode.left !== null) {
            stack.push(currentNode.left);
        }
    }

    return result;
}
```

## Approach 3: Iterative Preorder Traversal Using Stack

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used

function preorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    let current = root;

    while (current !== null) {
        if (current.left === null) {
            result.push(current.val); // Visit the root
            current = current.right;  // Move to the right
        } else {
            let predecessor = current.left;

            // Find the rightmost node in the left subtree
            while (predecessor.right !== null && predecessor.right !== current) {
                predecessor = predecessor.right;
            }

            if (predecessor.right === null) {
                result.push(current.val); // Visit the root
                predecessor.right = current; // Create a temporary thread to the root
                current = current.left; // Move to the left
            } else {
                predecessor.right = null; // Remove the temporary thread
                current = current.right; // Move to the right
            }
        }
    }

    return result;
}
```

