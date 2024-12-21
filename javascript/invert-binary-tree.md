# [LeetCode 226: Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

## Approaches
- [Recursive Depth-First Approach](#recursive-depth-first-approach)
- [Iterative Breadth-First Approach](#iterative-breadth-first-approach)

### Recursive Depth-First Approach

**Intuition:**

The recursive approach leverages the nature of trees where each tree node can be considered as its own "root" of a subtree. The idea here is straightforward: for each node in the tree, swap its left and right child, then apply this recursively to the left and right children of the node. This is akin to a post-order traversal where you visit left and right children before processing the node itself.

**Code:**

```javascript
function invertTree(root) {
    // Base case: If the node is null, there is nothing to invert
    if (root === null) {
        return null;
    }

    // Swap the left and right children of the current node
    let temp = root.left;
    root.left = root.right;
    root.right = temp;

    // Recursively invert the left subtree
    invertTree(root.left);

    // Recursively invert the right subtree
    invertTree(root.right);

    // Return the root of the inverted subtree
    return root;
}
```

**Time Complexity:** O(n)  
- We visit each node exactly once, where n is the number of nodes in the tree.

**Space Complexity:** O(h)  
- This is the space used by the recursion stack. "h" is the height of the tree, which in the worst case is O(n) for a skewed tree.

### Iterative Breadth-First Approach

**Intuition:**

Another way to tackle this problem is by using an iterative approach with a queue, simulating a level-order traversal (Breadth-First Search). At each node, we swap its left and right children. This approach avoids the recursive call stack overhead and might be more suitable for trees with large depth.

**Code:**

```javascript
function invertTree(root) {
    // Edge case: If the tree is empty, return the same
    if (root === null) {
        return null;
    }

    // Initialize a queue with the root node for BFS
    const queue = [root];
  
    // While there are nodes to process in the queue
    while (queue.length > 0) {
        // Dequeue the front node for processing
        let current = queue.shift();

        // Swap the left and right children of the current node
        let temp = current.left;
        current.left = current.right;
        current.right = temp;

        // Add the left child to the queue if it exists
        if (current.left !== null) {
            queue.push(current.left);
        }

        // Add the right child to the queue if it exists
        if (current.right !== null) {
            queue.push(current.right);
        }
    }
  
    // Return the root of the inverted tree
    return root;
}
```

**Time Complexity:** O(n)  
- Every node is visited and processed exactly once in the BFS traversal.

**Space Complexity:** O(n)  
- In the worst case, the space complexity is O(n), due to the storage of node references in the queue at the lower levels of a balanced tree where the last level might have n/2 nodes.

