# [Leetcode 783: Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes)

## Navigation
- [Approach 1: Brute Force using Inorder Traversal and Array](#approach-1-brute-force-using-inorder-traversal-and-array)
- [Approach 2: Optimized Inorder Traversal with Constant Space](#approach-2-optimized-inorder-traversal-with-constant-space)

## Approach 1: Brute Force using Inorder Traversal and Array

### Intuition
In a Binary Search Tree (BST), an inorder traversal gives nodes in sorted order. By leveraging this property, the problem of finding the minimum distance between any two nodes can be reduced to finding the minimum difference between successive elements in the sorted sequence.

### Steps
1. **Inorder Traversal**: Collect all the node values in a list using an inorder traversal.
2. **Calculate Minimum Difference**: Iterate through the sorted list of node values to determine the minimum absolute difference between successive elements.

### Code
```javascript
function minDiffInBST(root) {
    // Helper function to perform inorder traversal
    const inorderTraversal = (node, nodes) => {
        if (!node) return;
        inorderTraversal(node.left, nodes);
        nodes.push(node.val);  // Collect node's value
        inorderTraversal(node.right, nodes);
    }
    
    let nodes = [];
    inorderTraversal(root, nodes);
    
    let minDiff = Infinity;
    for (let i = 1; i < nodes.length; i++) {
        minDiff = Math.min(minDiff, nodes[i] - nodes[i - 1]);
    }
    return minDiff;
}
```

### Complexity
- **Time Complexity**: O(N), where N is the number of nodes in the BST, because we process each node once in the inorder traversal.
- **Space Complexity**: O(N), as we store each node's value in the array.

## Approach 2: Optimized Inorder Traversal with Constant Space

### Intuition
We can improve the space efficiency by calculating the minimum difference directly during the inorder traversal, thus maintaining a constant space usage. Instead of storing all node values, we keep track of only the previous node's value and compute the difference with the current node.

### Steps
1. **Inorder Traversal with Tracking**: Perform an inorder traversal while keeping track of the previously visited node's value.
2. **Update Minimum Distance**: Calculate and update the minimum difference on-the-fly as we traverse the nodes in sorted order.

### Code
```javascript
function minDiffInBST(root) {
    let prev = null;  // Will store the value of the previous node in inorder traversal
    let minDiff = Infinity;  // Initialize minimum difference to a large value

    const inorderTraversal = (node) => {
        if (!node) return;
        inorderTraversal(node.left);

        // Calculate min difference using the current node value and previously visited node value
        if (prev !== null) {
            minDiff = Math.min(minDiff, node.val - prev);
        }
        prev = node.val;  // Update prev to the current node value

        inorderTraversal(node.right);
    }

    inorderTraversal(root);
    return minDiff;
}
```

### Complexity
- **Time Complexity**: O(N), since each node is visited once during the traversal.
- **Space Complexity**: O(H), where H is the height of the BST, due to the recursive call stack. For balanced BSTs, this is O(log N). In the worst case, it's O(N) for an unbalanced tree. This approach improves on the space needed for tracking the nodes.

