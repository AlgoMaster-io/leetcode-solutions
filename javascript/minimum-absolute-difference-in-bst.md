# [Leetcode 530: Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

## Approaches:
- [Approach 1: Brute Force with Inorder Array](#approach-1-brute-force-with-inorder-array)
- [Approach 2: Optimized Inorder Traversal Without Array](#approach-2-optimized-inorder-traversal-without-array)

### Approach 1: Brute Force with Inorder Array

**Intuition:**  
The most straightforward way to solve the problem is to use an inorder traversal to capture all the node values in a sorted array, and then calculate the minimum difference between consecutive elements in this array. Since the inorder traversal of a BST produces a sorted sequence, we can exploit this property.

**Algorithm:**
1. Perform an inorder traversal of the BST to store the values in an array.
2. Iterate over the array to determine the minimum difference between consecutive elements.

```javascript
function getMinimumDifference(root) {
    const inorder = (node, arr) => {
        if (!node) return;
        inorder(node.left, arr);
        arr.push(node.val); // Collect node values in sorted order
        inorder(node.right, arr);
    };
    
    const values = [];
    inorder(root, values);

    let minDiff = Infinity;
    for (let i = 1; i < values.length; i++) {
        minDiff = Math.min(minDiff, values[i] - values[i - 1]); // Compute minimum difference
    }
    
    return minDiff;
}
```

**Time Complexity:** O(N), where N is the number of nodes, due to the inorder traversal.  
**Space Complexity:** O(N), for the array that stores the node values.

### Approach 2: Optimized Inorder Traversal Without Array

**Intuition:**  
We can optimize the space usage by keeping track of the previous value during the inorder traversal without storing all values in an array. This can be done by maintaining a variable to hold the previous node's value and updating the minimum difference on-the-go.

**Algorithm:**
1. Use a recursive helper function to perform an inorder traversal.
2. During the traversal, calculate the absolute difference between the current node's value and the previous node's value.
3. Update the minimum difference as needed.

```javascript
function getMinimumDifference(root) {
    let prev = null;
    let minDiff = Infinity;

    const inorder = (node) => {
        if (!node) return;

        inorder(node.left);

        if (prev !== null) {
            minDiff = Math.min(minDiff, node.val - prev); // Update minimum difference using current and previous values
        }
        prev = node.val; // Update previous value to current node value

        inorder(node.right);
    };

    inorder(root);
    return minDiff;
}
```

**Time Complexity:** O(N), where N is the number of nodes, due to the traversal of the tree.  
**Space Complexity:** O(H), where H is the height of the tree, due to the recursion stack. In the worst case of a skewed tree, this can be O(N). 

This optimized approach provides a more efficient solution in terms of space while maintaining a straightforward logic by leveraging properties inherent to a BST and inorder traversal.

