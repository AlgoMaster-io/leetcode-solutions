# [Leetcode 230: Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

## Table of Contents
- [Approach 1: In-Order Traversal with Array](#approach-1-in-order-traversal-with-array)
- [Approach 2: In-Order Traversal with Counter](#approach-2-in-order-traversal-with-counter)

## Approach 1: In-Order Traversal with Array

### Intuition
A Binary Search Tree (BST) has the property that in-order traversal of the tree results in sorted order of elements. We can leverage this property to find the kth smallest element by performing an in-order traversal and storing elements in an array. The kth element in this array will be our answer.

### Steps
1. Perform an in-order traversal of the BST.
2. Store the elements in an array during the traversal.
3. Access the (k-1)th index of this sorted array to get the kth smallest element.

### Code
```javascript
var kthSmallest = function(root, k) {
    // Array to store the elements from the in-order traversal
    let elements = [];
    
    // Helper function for in-order traversal
    function inOrder(node) {
        if (!node) return;
        inOrder(node.left);   // Traverse left subtree
        elements.push(node.val); // Visit current node
        inOrder(node.right);  // Traverse right subtree
    }
    
    // Perform the in-order traversal
    inOrder(root);
    
    // Return the k-th smallest element (1-indexed)
    // In zero-indexed terms, it would be (k-1)th position
    return elements[k - 1];
};
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the tree. We traverse each node once.
- **Space Complexity**: O(N), for storing the elements from the in-order traversal.

## Approach 2: In-Order Traversal with Counter

### Intuition
Instead of storing all elements, we can directly keep count of nodes visited and stop once we reach the kth element. This approach is more space-efficient than storing all elements in an array.

### Steps
1. Perform in-order traversal using a recursive helper function.
2. Maintain a counter to track the number of nodes visited.
3. As soon as the counter reaches k, store the node's value as the answer and stop further traversal.

### Code
```javascript
var kthSmallest = function(root, k) {
    let count = 0;  // Number of nodes visited so far
    let result = null; // Result to store the k-th smallest value

    // Helper function for in-order traversal
    function inOrder(node) {
        if (!node || result !== null) return; // base case: null node or found result

        inOrder(node.left); // Traverse left subtree

        count++; // Increment the count for this node
        if (count === k) {
            result = node.val; // Store result if it's the k-th smallest
            return;
        }

        inOrder(node.right); // Traverse right subtree
    }
    
    inOrder(root); // Start in-order traversal
    
    return result; // Return the k-th smallest element
};
```

### Complexity Analysis
- **Time Complexity**: O(H + k), where H is the height of the tree. In the worst-case scenario, we might traverse down to the leaf nodes and then start counting to k.
- **Space Complexity**: O(H), due to the recursion stack in the case of a completely unbalanced tree. In a balanced tree, this reduces to O(log N).

