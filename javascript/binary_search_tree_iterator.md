# 173. [Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approach 1: Controlled Recursion Using Stack

### Solution
```javascript
// Time Complexity:
//   - Constructor: O(h), where h is the height of the tree
//   - next(): O(1) on average across all calls
//   - hasNext(): O(1)
// Space Complexity: O(h), where h is the height of the tree for the stack

class BSTIterator {
    constructor(root) {
        this.stack = [];
        this.pushLeft(root);
    }

    // Push all left children of the current node to the stack
    pushLeft(node) {
        while (node !== null) {
            this.stack.push(node);
            node = node.left;
        }
    }

    /** @return the next smallest number */
    next() {
        const node = this.stack.pop(); // Get the topmost node (smallest available)
        if (node.right !== null) {
            this.pushLeft(node.right); // Process the right subtree
        }
        return node.val;
    }

    /** @return whether we have a next smallest number */
    hasNext() {
        return this.stack.length > 0; // Check if the stack is non-empty
    }
}
```

## Approach 2: Precomputed Inorder Traversal

### Solution
```javascript
// Time Complexity:
//   - Constructor: O(n), where n is the number of nodes in the tree
//   - next(): O(1)
//   - hasNext(): O(1)
// Space Complexity: O(n), where n is the number of nodes in the tree

class BSTIterator {
    constructor(root) {
        this.inorder = [];
        this.index = 0;
        this.inorderTraversal(root);
    }

    // Perform an inorder traversal to store the elements
    inorderTraversal(node) {
        if (node === null) {
            return;
        }
        this.inorderTraversal(node.left);
        this.inorder.push(node.val);
        this.inorderTraversal(node.right);
    }

    /** @return the next smallest number */
    next() {
        return this.inorder[this.index++];
    }

    /** @return whether we have a next smallest number */
    hasNext() {
        return this.index < this.inorder.length;
    }
}
```

## Approach 3: Morris Traversal (Space Optimized, Less Practical for Iterator)

### Solution
```javascript
// Time Complexity:
//   - Constructor: O(1) (no precomputation)
//   - next(): O(1) on average across all calls
//   - hasNext(): O(1)
// Space Complexity: O(1), no additional storage

class BSTIterator {
    constructor(root) {
        this.current = root;
        this.predecessor = null;
    }

    /** @return the next smallest number */
    next() {
        let value = -1;

        while (this.current !== null) {
            if (this.current.left === null) {
                value = this.current.val;
                this.current = this.current.right; // Move to the right subtree
                break;
            } else {
                this.predecessor = this.current.left;

                // Find the rightmost node in the left subtree
                while (this.predecessor.right !== null && this.predecessor.right !== this.current) {
                    this.predecessor = this.predecessor.right;
                }

                if (this.predecessor.right === null) {
                    this.predecessor.right = this.current; // Create a temporary link
                    this.current = this.current.left;
                } else {
                    this.predecessor.right = null; // Remove the temporary link
                    value = this.current.val;
                    this.current = this.current.right; // Move to the right subtree
                    break;
                }
            }
        }

        return value;
    }

    /** @return whether we have a next smallest number */
    hasNext() {
        return this.current !== null;
    }
}
```

