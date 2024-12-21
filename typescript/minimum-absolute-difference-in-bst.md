# [LeetCode 530: Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

### Table of Contents
- [Approach 1: In-order Traversal with Array Storage](#approach-1)
- [Approach 2: Optimized In-Order Traversal with Constant Space](#approach-2)

## Approach 1: In-order Traversal with Array Storage <a name="approach-1"></a>

### Intuition:
The basic idea is to perform an in-order traversal of the BST which naturally provides us with the values in ascending order. By storing the node values in an array, we can simply iterate through it to find the minimum absolute difference.

### Steps:
1. Perform an in-order traversal of the BST and store the values in a sorted array.
2. Iterate over the sorted array and calculate differences between consecutive elements.
3. Return the minimum difference found.

### Time Complexity:
- **O(n)**: We traverse the tree once to collect the values.
  
### Space Complexity:
- **O(n)**: We use additional space to store the node values.

### Code:
```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.left = (left === undefined ? null : left);
        this.right = (right === undefined ? null : right);
    }
}

function getMinimumDifference(root: TreeNode | null): number {
    // Array to store the in-order traversal of the tree
    const values: number[] = [];

    // Helper function to perform the in-order traversal
    function inOrder(node: TreeNode | null) {
        if (node === null) return;
        inOrder(node.left);
        values.push(node.val); // Collecting the node value
        inOrder(node.right);
    }

    inOrder(root); // Start in-order traversal from the root

    let minDifference = Infinity; // Initialize minimum difference to infinity

    // Calculate the minimum difference between consecutive elements in the sorted array
    for (let i = 1; i < values.length; i++) {
        minDifference = Math.min(minDifference, values[i] - values[i - 1]);
    }

    return minDifference;
}
```

## Approach 2: Optimized In-Order Traversal with Constant Space <a name="approach-2"></a>

### Intuition:
The primary optimization is to conduct the in-order traversal and calculate the minimum difference on-the-fly, without storing all node values. This approach reduces the space complexity to O(h), where h is the height of the tree (space used by the recursion stack).

### Steps:
1. Perform in-order traversal while keeping track of the previous node visited.
2. Calculate the difference between the current and previous node values during the traversal.
3. Update the minimum difference found.

### Time Complexity:
- **O(n)**: We still traverse each node once.

### Space Complexity:
- **O(h)**: Space used by recursion stack where h is the height of the tree.

### Code:
```typescript
function getMinimumDifferenceOptimized(root: TreeNode | null): number {
    let minDifference = Infinity; // Initialize the minimum difference to infinity
    let prevNode: TreeNode | null = null; // To keep track of the previous node in in-order traversal

    // Helper function to perform the in-order traversal
    function inOrder(node: TreeNode | null) {
        if (node === null) return;
        inOrder(node.left); // Visit the left subtree

        // If we have a previous node, calculate the difference and update minimum difference
        if (prevNode !== null) {
            minDifference = Math.min(minDifference, node.val - prevNode.val);
        }

        prevNode = node; // Update the previous node to current node
        inOrder(node.right); // Visit the right subtree
    }

    inOrder(root); // Start in-order traversal from the root

    return minDifference;
}
```

Each approach provides insights into utilizing BST properties and recursive traversal strategies to solve the problem, with the second approach being more memory efficient while maintaining optimal time complexity.

