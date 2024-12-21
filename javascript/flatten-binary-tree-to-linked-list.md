# [Leetcode 114: Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach with a Stack](#iterative-approach-with-a-stack)
3. [Optimized Space Approach: Morris Traversal](#optimized-space-approach-morris-traversal)

## Recursive Approach

### Intuition:
This approach uses a post-order traversal to flatten the tree from bottom-up. The idea is to flatten the left and right subtrees recursively, then attach the flattened left subtree between the root and the flattened right subtree.

### Code:
```javascript
var flatten = function(root) {
    const flattenTree = (node) => {
        if (node === null) return;
        
        // Recursively flatten the left and right subtrees
        flattenTree(node.left);
        flattenTree(node.right);

        // Store the left and right nodes
        let left = node.left;
        let right = node.right;

        // Make the left subtree become the right subtree
        node.left = null;
        node.right = left;

        // Attach the original right subtree at the end of new right subtree
        let current = node;
        while (current.right !== null) {
            current = current.right;
        }
        current.right = right;
    };
    
    flattenTree(root);
};
```

### Complexity:
- **Time Complexity**: O(N), where N is the number of nodes. We visit each node exactly once.
- **Space Complexity**: O(H), where H is the height of the tree. This space is used in the function call stack during recursion.

## Iterative Approach with a Stack

### Intuition:
Using a stack can simulate the post-order traversal iteratively. We process nodes in a manner similar to pre-order traversal, using a stack to guide the traversal order and modifying the tree as we go.

### Code:
```javascript
var flatten = function(root) {
    if (root === null) return;
    
    const stack = [root];
    while (stack.length > 0) {
        let currentNode = stack.pop();
        
        // Push right and left children of the node to the stack
        if (currentNode.right !== null) {
            stack.push(currentNode.right);
        }
        if (currentNode.left !== null) {
            stack.push(currentNode.left);
        }
        
        // Reassign pointers
        if (stack.length > 0) {
            currentNode.right = stack[stack.length - 1];
        }
        currentNode.left = null; // Set left to null as per the problem definition
    }
};
```

### Complexity:
- **Time Complexity**: O(N), where N is the number of nodes. Each node is processed once.
- **Space Complexity**: O(N), in the worst case, when the tree is a complete binary tree, we might have all nodes in the stack.

## Optimized Space Approach: Morris Traversal

### Intuition:
Morris Traversal is used to flatten the tree in place without using a stack or recursion. It works by temporarily modifying the tree structure. For each node, find its predecessor, then link its predecessor's right child to the current node's right child.

### Code:
```javascript
var flatten = function(root) {
    let current = root;
    while (current !== null) {
        if (current.left !== null) {
            // Find the rightmost in the left subtree
            let predecessor = current.left;
            while (predecessor.right !== null) {
                predecessor = predecessor.right;
            }
            
            // Link the rightmost to the current node's right subtree
            predecessor.right = current.right;

            // Move the left subtree to the right
            current.right = current.left;
            current.left = null; // Set left to null as per the problem definition
        }
        current = current.right; // Move to the right
    }
};
```

### Complexity:
- **Time Complexity**: O(N), since each node is visited and modified once.
- **Space Complexity**: O(1), as we do not use any auxiliary data structures, and modifications are made in place.

