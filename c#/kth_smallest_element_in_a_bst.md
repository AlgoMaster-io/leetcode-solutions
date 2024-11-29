# 230. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

## Approach 1: Inorder Traversal (Recursive)

### Solution
c#
// Time Complexity: O(n), where n is the number of nodes in the tree (in the worst case)
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
public class Solution {
    private int count = 0;
    private int result = 0;

    public int KthSmallest(TreeNode root, int k) {
        Inorder(root, k);
        return result;
    }

    private void Inorder(TreeNode node, int k) {
        if (node == null) {
            return;
        }

        Inorder(node.left, k); // Recurse for the left subtree

        count++; // Increment the count for each visited node
        if (count == k) {
            result = node.val; // Store the kth smallest value
            return;
        }

        Inorder(node.right, k); // Recurse for the right subtree
    }
}

## Approach 2: Inorder Traversal (Iterative)

### Solution
c#
// Time Complexity: O(k), where k is the kth smallest element to be found
// Space Complexity: O(h), where h is the height of the tree for the stack
using System.Collections.Generic;

public class Solution {
    public int KthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode current = root;
        int count = 0;

        while (current != null || stack.Count > 0) {
            while (current != null) {
                stack.Push(current); // Push nodes of the left subtree
                current = current.left;
            }

            current = stack.Pop(); // Process the top node
            count++;

            if (count == k) {
                return current.val; // Return the kth smallest value
            }

            current = current.right; // Move to the right subtree
        }

        return -1; // This line should never be reached if k is valid
    }
}

## Approach 3: Binary Search on BST Property

### Solution
c#
// Time Complexity: O(h + k), where h is the height of the tree and k is the position of the kth smallest element
// Space Complexity: O(h), where h is the height of the tree for recursion stack
public class Solution {
    public int KthSmallest(TreeNode root, int k) {
        int leftNodeCount = CountNodes(root.left);

        if (leftNodeCount == k - 1) {
            return root.val; // Root is the kth smallest element
        } else if (leftNodeCount >= k) {
            return KthSmallest(root.left, k); // Recurse on the left subtree
        } else {
            return KthSmallest(root.right, k - leftNodeCount - 1); // Recurse on the right subtree
        }
    }

    private int CountNodes(TreeNode node) {
        if (node == null) {
            return 0;
        }
        return 1 + CountNodes(node.left) + CountNodes(node.right); // Count nodes in the subtree
    }
}

