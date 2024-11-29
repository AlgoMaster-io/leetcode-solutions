# 230. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

## Approach 1: Inorder Traversal (Recursive)

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree (in the worst case)
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
class Solution {
    constructor() {
        this.count = 0;
        this.result = 0;
    }

    kthSmallest(root, k) {
        this.inorder(root, k);
        return this.result;
    }

    inorder(node, k) {
        if (node === null) {
            return;
        }

        this.inorder(node.left, k); // Recurse for the left subtree

        this.count++; // Increment the count for each visited node
        if (this.count === k) {
            this.result = node.val; // Store the kth smallest value
            return;
        }

        this.inorder(node.right, k); // Recurse for the right subtree
    }
}
```

## Approach 2: Inorder Traversal (Iterative)

### Solution
```javascript
// Time Complexity: O(k), where k is the kth smallest element to be found
// Space Complexity: O(h), where h is the height of the tree for the stack
class Solution {
    kthSmallest(root, k) {
        let stack = [];
        let current = root;
        let count = 0;

        while (current !== null || stack.length !== 0) {
            while (current !== null) {
                stack.push(current); // Push nodes of the left subtree
                current = current.left;
            }

            current = stack.pop(); // Process the top node
            count++;

            if (count === k) {
                return current.val; // Return the kth smallest value
            }

            current = current.right; // Move to the right subtree
        }

        return -1; // This line should never be reached if k is valid
    }
}
```

## Approach 3: Binary Search on BST Property

### Solution
```javascript
// Time Complexity: O(h + k), where h is the height of the tree and k is the position of the kth smallest element
// Space Complexity: O(h), where h is the height of the tree for recursion stack
class Solution {
    kthSmallest(root, k) {
        const leftNodeCount = this.countNodes(root.left);

        if (leftNodeCount === k - 1) {
            return root.val; // Root is the kth smallest element
        } else if (leftNodeCount >= k) {
            return this.kthSmallest(root.left, k); // Recurse on the left subtree
        } else {
            return this.kthSmallest(root.right, k - leftNodeCount - 1); // Recurse on the right subtree
        }
    }

    countNodes(node) {
        if (node === null) {
            return 0;
        }
        return 1 + this.countNodes(node.left) + this.countNodes(node.right); // Count nodes in the subtree
    }
}
```

