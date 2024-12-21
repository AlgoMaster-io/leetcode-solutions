# [Leetcode 783: Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

## Approaches
- [Approach 1: Inorder Traversal with Array Storage](#approach-1-inorder-traversal-with-array-storage)
- [Approach 2: Inorder Traversal with Previous Variable](#approach-2-inorder-traversal-with-previous-variable)

## Approach 1: Inorder Traversal with Array Storage

### Intuition
A Binary Search Tree (BST) has the property that an in-order traversal visits the nodes in ascending order. We can leverage this property to find the minimum difference between nodes. By performing an in-order traversal, we can store the node values in an array. Then, by iterating over the array, we can find the minimum difference between consecutive elements.

### Steps
1. Perform an in-order traversal of the BST.
2. Store the values of the nodes during the traversal in an array.
3. Traverse the sorted array and compute the difference between consecutive elements to find the minimum difference.

### Code
```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val)
        this.left = (left===undefined ? null : left)
        this.right = (right===undefined ? null : right)
    }
}

function minDiffInBST(root: TreeNode | null): number {
    // Array to store the values from the in-order traversal
    const values: number[] = [];

    // Helper function to perform an in-order traversal
    function inorder(node: TreeNode | null) {
        if (!node) return;
        inorder(node.left); // Recursively traverse the left subtree
        values.push(node.val); // Store the current node's value
        inorder(node.right); // Recursively traverse the right subtree
    }

    inorder(root);

    let minDiff = Infinity;
    for (let i = 1; i < values.length; i++) {
        // Compute the difference between consecutive node values
        minDiff = Math.min(minDiff, values[i] - values[i - 1]);
    }

    return minDiff;
}
```

### Complexity
- **Time Complexity**: O(N), where N is the number of nodes in the BST. We visit each node once during the traversal, and we also iterate over the array of node values.
- **Space Complexity**: O(N), for storing the values of the nodes in an array.

## Approach 2: Inorder Traversal with Previous Variable

### Intuition
Instead of storing all the node values in an array, we can keep a track of the previous node value during the in-order traversal. This way, we can calculate the difference on-the-fly without having to store the entire tree.

### Steps
1. Use a variable to keep track of the previous node value encountered during the in-order traversal.
2. As we traverse the tree, calculate the minimum difference between the current and previous node values.

### Code
```typescript
function minDiffInBST(root: TreeNode | null): number {
    let prev: number | null = null;
    let minDiff = Infinity;

    // Helper function to perform an in-order traversal
    function inorder(node: TreeNode | null) {
        if (!node) return;

        inorder(node.left); // Traverse the left subtree

        // If there is a previous value, compute the difference
        if (prev !== null) {
            minDiff = Math.min(minDiff, node.val - prev);
        }
        prev = node.val; // Update the previous value to the current node's value

        inorder(node.right); // Traverse the right subtree
    }

    inorder(root);

    return minDiff;
}
```

### Complexity
- **Time Complexity**: O(N), where N is the number of nodes in the BST, as each node is visited once.
- **Space Complexity**: O(H), where H is the height of the tree. This space is used by the recursion stack. In the worst case of an unbalanced tree, this may approach O(N). For a balanced tree, it would be O(log N).

