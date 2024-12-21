# [Leetcode 108: Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

## Approaches
- [Approach 1: Recursive Divide and Conquer](#approach-1-recursive-divide-and-conquer)

## Approach 1: Recursive Divide and Conquer

### Intuition
Given a sorted array, the goal is to convert it into a height-balanced binary search tree (BST). A height-balanced BST is a tree in which the depth of the two subtrees of every node never differs by more than 1. To achieve this, the middle element of the array can be chosen as the root node of the BST. This ensures that the left and the right subtrees have nearly equal numbers of nodes, which helps maintain balance. Recursively, this method can be applied to the subarrays on the left and right of the middle element to construct the entire tree.

### Code
```csharp
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int x) { val = x; }
}

public class Solution {
    public TreeNode SortedArrayToBST(int[] nums) {
        // Helper function to recurively build the BST
        TreeNode BuildBST(int left, int right) {
            // Base case: If left exceeds right, we do not have a valid sub-array
            if (left > right) {
                return null;
            }
            
            // Choose the middle element to maintain balance
            int mid = left + (right - left) / 2; // To prevent integer overflow

            // Create the current tree node
            TreeNode node = new TreeNode(nums[mid]);
            
            // Recursively build the left subtree on the left subarray
            node.left = BuildBST(left, mid - 1);
            
            // Recursively build the right subtree on the right subarray
            node.right = BuildBST(mid + 1, right);
            
            return node;
        }
        
        // Initiate recursion starting with the whole array range
        return BuildBST(0, nums.Length - 1);
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of elements in the input array. Each element is visited once to build the tree.
- **Space Complexity**: O(log n), which accounts for the space used by the recursive function call stack. This is the height of the tree, and log n is the balanced tree's height for n elements in the worst case.

This approach efficiently converts a sorted array into a height-balanced BST by using a recursive, divide-and-conquer strategy. The core idea revolves around choosing the middle element as the root to ensure balanced divisions.

