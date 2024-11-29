# 98. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Approach 1: Inorder Traversal (Iterative)

### Solution
javascript
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack

function isValidBST(root) {
    const stack = [];
    let current = root;
    let prev = null;

    while (current !== null || stack.length > 0) {
        while (current !== null) {
            stack.push(current); // Push nodes of the left subtree
            current = current.left;
        }

        current = stack.pop(); // Process the top node

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
javascript
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack

function isValidBST(root) {
    let prev = null;

    function inorder(node) {
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
javascript
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack

function isValidBST(root) {
    function validate(node, lower, upper) {
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

