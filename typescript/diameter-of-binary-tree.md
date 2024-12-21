# [Leetcode 543: Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

## Table of Contents
- [Basic Solution: Recursion with Depth Calculation](#basic-solution-recursion-with-depth-calculation)
- [Optimized Solution: Single Pass Calculation](#optimized-solution-single-pass-calculation)

## Basic Solution: Recursion with Depth Calculation

### Intuition
The **diameter** of a binary tree is defined as the longest path between any two nodes in the tree. The path does not necessarily pass through the root node.

A simple solution is to go through each node and calculate the diameter using that node as the root of the subtree. The longest path at any node is the sum of the height of its left and right subtrees.

We'll define a helper function to compute the height of a subtree and use it to calculate the diameter at each node.

### Code

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.left = (left===undefined ? null : left);
        this.right = (right===undefined ? null : right);
    }
}

function diameterOfBinaryTree(root: TreeNode | null): number {
    let diameter = 0;

    function height(node: TreeNode | null): number {
        if (node === null) {
            return 0; // The height of an empty tree is 0
        }
        
        // Recursively find the height of the left and right subtrees
        const leftHeight = height(node.left); 
        const rightHeight = height(node.right);
        
        // The potential diameter at this node is the sum of the heights of the subtrees
        const potentialDiameter = leftHeight + rightHeight;
        
        // Update the global diameter if this is the largest we've seen
        diameter = Math.max(diameter, potentialDiameter);
        
        // Return the height of this subtree
        return Math.max(leftHeight, rightHeight) + 1;
    }

    height(root);
    return diameter;
}
```

### Complexity Analysis
- **Time Complexity:** O(N), where N is the number of nodes in the tree. We come to each node once and do a constant amount of work (computing height and updating diameter).
- **Space Complexity:** O(H), where H is the height of the tree. This is due to recursive call stack usage.

## Optimized Solution: Single Pass Calculation

### Intuition
The above solution does an unnecessary recomputation of the height, since every node recalculates the height of its subtrees. We can optimize this by using a single recursive traversal where we calculate both the diameter and the height in the same pass.

### Code

```typescript
function diameterOfBinaryTree(root: TreeNode | null): number {
    let diameter = 0;

    function longestPath(node: TreeNode | null): number {
        if (node === null) {
            return 0; // The height of an empty tree is 0
        }

        // Compute the longest path in the left subtree
        const leftPath = longestPath(node.left);
        // Compute the longest path in the right subtree
        const rightPath = longestPath(node.right);

        // Path can be the longest from left to right including this node
        diameter = Math.max(diameter, leftPath + rightPath);
        
        // Return the longest one that includes this node and one child
        return Math.max(leftPath, rightPath) + 1;
    }

    longestPath(root);
    return diameter;
}
```

### Complexity Analysis
- **Time Complexity:** O(N), where N is the number of nodes in the tree. We traverse every node once calculating the required longest paths.
- **Space Complexity:** O(H), where H is the height of the tree due to the recursive call stack.
  
This approach efficiently consolidates the operations to minimize repetition and improve performance.

