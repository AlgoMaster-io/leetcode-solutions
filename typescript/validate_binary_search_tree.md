# 98. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Approach 1: Inorder Traversal (Iterative)

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
        this.val = val === undefined ? 0 : val;
        this.left = left === undefined ? null : left;
        this.right = right === undefined ? null : right;
    }
}

function isValidBST(root: TreeNode | null): boolean {
    const stack: TreeNode[] = [];
    let current = root;
    let prev: TreeNode | null = null;

    while (current !== null || stack.length > 0) {
        while (current !== null) {
            stack.push(current); // Push nodes of the left subtree
            current = current.left;
        }

        current = stack.pop()!; // Process the top node

        if (prev !== null && current.val <= prev.val) {
            return false; // If the current value is not greater than the previous, it's invalid
        }
        prev = current;

        current = current.right; // Move to the right subtree
    }

    return true;
}
```

## Approach 2: Recursive Inorder Traversal

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack

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

function isValidBST(root: TreeNode | null): boolean {
    let prev: TreeNode | null = null;

    function inorder(node: TreeNode | null): boolean {
        if (node === null) {
            return true; // Base case: empty tree is valid
        }

        if (!inorder(node.left)) {
            return false; // Left subtree is invalid
        }

        if (prev !== null && node.val <= prev.val) {
            return false; // Current value is not greater than the previous
        }
        prev = node; // Update the previous node

        return inorder(node.right); // Recurse for the right subtree
    }

    return inorder(root);
}
```

## Approach 3: DFS with Range Validation

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack

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

function isValidBST(root: TreeNode | null): boolean {
    function validate(node: TreeNode | null, lower: number | null, upper: number | null): boolean {
        if (node === null) {
            return true; // Base case: empty tree is valid
        }

        if ((lower !== null && node.val <= lower) || (upper !== null && node.val >= upper)) {
            return false; // Node value violates the range constraints
        }

        // Recursively validate the left and right subtrees
        return validate(node.left, lower, node.val) && validate(node.right, node.val, upper);
    }

    return validate(root, null, null);
}
```

