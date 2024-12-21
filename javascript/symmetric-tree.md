# [Leetcode 101: Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

In this problem, we are asked to determine if a binary tree is symmetric around its center. A binary tree is symmetric if the left subtree is a mirror reflection of the right subtree.

### Approaches:

- [Recursive Approach](#recursive-approach)
- [Iterative Approach with a Queue](#iterative-approach-with-a-queue)

## Recursive Approach

The basic idea for the recursive approach is to verify if each subtree is a mirror reflection of itself. If the left and the right subtrees of a root are symmetric (mirror images), then the entire tree is symmetric.

### Intuition

1. If both subtrees are empty, they are symmetric.
2. If only one of the subtrees is empty, they are not symmetric.
3. If both subtrees are not empty, they must have the same value, and their respective children must be mirror images of each other.

### Steps

- Define a recursive helper function `isMirror` that:
  - Checks if two given nodes are mirror images.
  - If they are not, return `false`.
  - Recursively check if the right child of the first node is a mirror of the left child of the second node and vice versa.

```javascript
function isSymmetric(root) {
    if (!root) return true;
    
    function isMirror(left, right) {
        if (!left && !right) return true; // Both nodes are null, symmetric
        if (!left || !right) return false; // One node is null, not symmetric
        if (left.val !== right.val) return false; // Values are different, not symmetric
        
        // Compare outer and inner sides of subtrees
        return isMirror(left.left, right.right) && isMirror(left.right, right.left);
    }

    return isMirror(root.left, root.right);
}
```

### Time Complexity

- **Time Complexity**: O(n), where 'n' is the number of nodes in the tree. This is because every node is checked once.
- **Space Complexity**: O(h), where 'h' is the height of the tree due to the recursion stack. In the worst-case scenario, the tree is completely unbalanced, requiring O(n) space (skewed tree).

## Iterative Approach with a Queue

Using an iterative approach with a queue can avoid the recursion stack which could be advantageous in deeply nested trees, preventing a stack overflow.

### Intuition

1. Use a queue to compare nodes in pairs.
2. Initially enqueue the left and right children of the root.
3. While the queue is not empty:
   - Dequeue two nodes and compare them.
   - If they don't match, the tree is not symmetric.
   - If they are mirror images, enqueue their children in mirrored order to continue the comparison.

### Steps

- Use a queue to store nodes for processing.
- Dequeue nodes in pairs and compare them.
- Enqueue their children in mirrored order for future comparison.

```javascript
function isSymmetric(root) {
    if (!root) return true;
    
    let queue = [];
    queue.push(root.left, root.right);
    
    while (queue.length > 0) {
        let left = queue.shift();
        let right = queue.shift();
        
        if (!left && !right) continue; // Both null, symmetric
        if (!left || !right) return false; // One is null, not symmetric
        if (left.val !== right.val) return false; // Different values, not symmetric
        
        // Enqueue children in mirror order
        queue.push(left.left, right.right);
        queue.push(left.right, right.left);
    }
    
    return true;
}
```

### Time Complexity

- **Time Complexity**: O(n), similar to the recursive approach, all nodes are checked.
- **Space Complexity**: O(n), as up to 'n' nodes can be stored in the queue in the worst case (level order traversal). 

Both approaches efficiently determine tree symmetry, allowing you to choose based on preference or constraints like stack depth concerns.

