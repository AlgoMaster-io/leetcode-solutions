# [Leetcode 783: Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

## Solutions Overview

- [Approach 1: Brute Force with Inorder Traversal](#approach-1-brute-force-with-inorder-traversal)
- [Approach 2: Optimized Inorder Traversal](#approach-2-optimized-inorder-traversal)

## Approach 1: Brute Force with Inorder Traversal

### Intuition

A binary search tree (BST) has the property that for any node, the value of all the nodes in its left subtree are less and all the nodes in its right subtree are greater. Thus, an inorder traversal of a BST will yield the nodes in sorted order.

The brute force approach is to perform an inorder traversal of the BST to obtain a sorted list of node values. We then compute the minimum difference between consecutive values in this sorted list.

### Steps

1. Perform an inorder traversal to extract values in sorted order.
2. Calculate the minimum difference between consecutive elements in this sorted list.

### Code

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int minDiffInBST(TreeNode root) {
        List<Integer> values = new ArrayList<>();
        inorderTraversal(root, values);
        
        // Initialize the minimum difference to a large value
        int minDiff = Integer.MAX_VALUE;
        
        // Compare differences between consecutive elements
        for (int i = 1; i < values.size(); i++) {
            minDiff = Math.min(minDiff, values.get(i) - values.get(i - 1));
        }
        return minDiff;
    }
    
    private void inorderTraversal(TreeNode node, List<Integer> values) {
        if (node == null) return;
        inorderTraversal(node.left, values);
        values.add(node.val); // Add current node value
        inorderTraversal(node.right, values);
    }
}
```

### Complexity Analysis

- **Time Complexity:** O(N) where N is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity:** O(N) for the auxiliary space used to store the values of nodes.

## Approach 2: Optimized Inorder Traversal

### Intuition

We can optimize the space usage by calculating the minimum difference on-the-fly during the inorder traversal, rather than maintaining a list of values. This way, we only need to keep track of the last visited node's value.

### Steps

1. Perform an inorder traversal.
2. During traversal, calculate the difference between the current node value and the previous node value (i.e., last node visited in inorder traversal).
3. Update the minimum difference accordingly.

### Code

```java
public class Solution {
    private int minDiff = Integer.MAX_VALUE;
    private Integer prev = null;
    
    public int minDiffInBST(TreeNode root) {
        inorder(root);
        return minDiff;
    }
    
    private void inorder(TreeNode node) {
        if (node == null) return;
        
        inorder(node.left);
        
        // Calculate the difference between current node and previous node
        if (prev != null) {
            minDiff = Math.min(minDiff, node.val - prev);
        }
        // Update previous node value to current node value
        prev = node.val;
        
        inorder(node.right);
    }
}
```

### Complexity Analysis

- **Time Complexity:** O(N) where N is the number of nodes. Still, we visit each node exactly once.
- **Space Complexity:** O(H) where H is the height of the tree. This accounts for the recursion stack during the depth-first traversal which, in the worst case, is the height of the tree.

By optimizing the traversal with inline calculation of differences, we achieve the same time complexity but reduce space usage significantly, making this approach preferred for large trees.

