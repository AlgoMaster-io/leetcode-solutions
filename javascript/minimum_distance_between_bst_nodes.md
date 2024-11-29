# 783. [Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

## Approach 1: Inorder Traversal with a List

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the inorder traversal

class Solution {
    minDiffInBST(root) {
        const values = [];
        this.inorder(root, values);

        let minDiff = Number.MAX_SAFE_INTEGER;
        for (let i = 1; i < values.length; i++) {
            minDiff = Math.min(minDiff, values[i] - values[i - 1]);
        }

        return minDiff;
    }

    inorder(node, values) {
        if (node === null) {
            return;
        }

        this.inorder(node.left, values); // Recurse for the left subtree
        values.push(node.val); // Add the current node value
        this.inorder(node.right, values); // Recurse for the right subtree
    }
}
```

## Approach 2: Inorder Traversal with Constant Space

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack

class Solution {
    constructor() {
        this.prev = null;
        this.minDiff = Number.MAX_SAFE_INTEGER;
    }

    minDiffInBST(root) {
        this.inorder(root);
        return this.minDiff;
    }

    inorder(node) {
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
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used

class Solution {
    minDiffInBST(root) {
        let current = root;
        let prev = null;
        let minDiff = Number.MAX_SAFE_INTEGER;

        while (current !== null) {
            if (current.left === null) {
                // Process the current node
                if (prev !== null) {
                    minDiff = Math.min(minDiff, current.val - prev.val);
                }
                prev = current;
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
}
```

