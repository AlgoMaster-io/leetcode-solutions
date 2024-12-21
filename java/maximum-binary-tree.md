# [Leetcode Problem 654: Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Approaches
1. [Recursive Divide and Conquer](#approach-1-recursive-divide-and-conquer)

---

## Approach 1: Recursive Divide and Conquer

### Intuition
The problem requires constructing a binary tree where the root is the maximum number in the list, the left subtree is constructed from the elements before this maximum number, and the right subtree is from the elements after this maximum number. This naturally suggests a recursive approach where at each step, we select the maximum element as the root and recursively construct the left and right subtrees from the remaining elements.

### Steps
1. **Base Case**: If the input list is empty, return `null` since there's no tree to be constructed.
2. **Find Maximum**: Locate the maximum value in the array and identify its index.
3. **Construct Node**: Create a tree node with the maximum value.
4. **Recursive Construction**:
    - Recursively construct the left subtree using the elements to the left of the maximum value.
    - Recursively construct the right subtree using elements to the right.
5. **Link Subtrees**: Assign the resulting subtrees to the left and right of the current node.

### Time and Space Complexity
- **Time Complexity**: O(n^2) in the worst case where the array is sorted and each recursive call will scan through the array slice to find the max. However, in average cases, this is closer to O(n log n).
- **Space Complexity**: O(n) to keep the recursion stack due to the depth of the recursion.

### Java Code
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

class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        // Start the recursive tree construction.
        return construct(nums, 0, nums.length);
    }
    
    private TreeNode construct(int[] nums, int left, int right) {
        // If the array slice is empty, return null.
        if (left == right) {
            return null;
        }
        
        // Find the index of the maximum element.
        int maxIndex = maxIndex(nums, left, right);
        
        // Create the root node with the maximum value.
        TreeNode root = new TreeNode(nums[maxIndex]);
        
        // Recursively construct the left subtree.
        root.left = construct(nums, left, maxIndex);
        
        // Recursively construct the right subtree.
        root.right = construct(nums, maxIndex + 1, right);
        
        // Return the constructed node.
        return root;
    }
    
    private int maxIndex(int[] nums, int left, int right) {
        int maxIndex = left;
        for (int i = left + 1; i < right; i++) {
            if (nums[i] > nums[maxIndex]) {
                maxIndex = i;
            }
        }
        return maxIndex;
    }
}
```

This code follows a divide-and-conquer strategy to build the Maximum Binary Tree by recursively finding the maximum number as the root for each subsection of the array, thereby constructing the entire binary tree.

