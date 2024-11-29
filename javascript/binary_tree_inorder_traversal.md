# 94. [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approach 1: Recursive Inorder Traversal

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

function inorderTraversal(root) {
    const result = [];
    inorder(root, result);
    return result;
}

function inorder(node, result) {
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
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack

function inorderTraversal(root) {
    const result = [];
    const stack = [];
    let current = root;

    while (current !== null || stack.length > 0) {
        while (current !== null) {
            stack.push(current); // Push the current node and move to the left subtree
            current = current.left;
        }

        current = stack.pop(); // Process the top node
        result.push(current.val); // Visit the node
        current = current.right; // Move to the right subtree
    }

    return result;
}
```

## Approach 3: Morris Inorder Traversal (Without Recursion or Stack)

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used

function inorderTraversal(root) {
    const result = [];
    let current = root;

    while (current !== null) {
        if (current.left === null) {
            result.push(current.val); // Visit the node
            current = current.right; // Move to the right subtree
        } else {
            let predecessor = current.left;

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

