# 94. [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approach 1: Recursive Inorder Traversal

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

function inorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    inorder(root, result);
    return result;
}

function inorder(node: TreeNode | null, result: number[]): void {
    if (node === null) {
        return; // Base case: if the node is null, return
    }

    inorder(node.left, result); // Recurse for the left subtree
    result.push(node.val); // Visit the root
    inorder(node.right, result); // Recurse for the right subtree
}
```

## Approach 2: Iterative Inorder Traversal Using Stack

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack

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

function inorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    const stack: TreeNode[] = [];
    let current: TreeNode | null = root;

    while (current !== null || stack.length !== 0) {
        while (current !== null) {
            stack.push(current); // Push the current node and move to the left subtree
            current = current.left;
        }

        current = stack.pop()!; // Process the top node
        result.push(current.val); // Visit the node
        current = current.right; // Move to the right subtree
    }

    return result;
}
```

## Approach 3: Morris Inorder Traversal (Without Recursion or Stack)

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used

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

function inorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    let current: TreeNode | null = root;

    while (current !== null) {
        if (current.left === null) {
            result.push(current.val); // Visit the node
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
                result.push(current.val); // Visit the node
                current = current.right; // Move to the right subtree
            }
        }
    }

    return result;
}
```

