# [Leetcode 108: Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

## Solutions

- [Approach 1: Recursive Binary Search with Middle Element](#approach-1-recursive-binary-search-with-middle-element)

---

### Approach 1: Recursive Binary Search with Middle Element

**Intuition:**

To convert a sorted array to a height-balanced binary search tree (BST), the ideal approach is to always select the middle element of the current array (or subarray) as the root. This choice ensures that the lengths of left and right subtrees are as balanced as possible.

The strategy here is to use a recursive approach where:
1. We identify the middle element of the current subarray.
2. Use this element as the root of the current subtree.
3. Recursively do the same for the left half to form the left subtree.
4. Recursively do the same for the right half to form the right subtree.

The base case is when the subarray becomes empty, at which point we return `nullptr`.

Here's a step-by-step breakdown:
- **Select the middle element** of the current subarray as the root.
- Recursively apply the same logic for the left half: `[left, mid-1]` and the right half: `[mid+1, right]`.

**Time Complexity:**
- O(n), where n is the number of elements in the array because each of the n elements is processed exactly once.

**Space Complexity:**
- O(log n) on average, due to the recursion stack, where n is the number of elements in the array. This is the height of the tree for a balanced BST.

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

class Solution {
public:
    // Helper function to convert a sorted subarray to a balanced BST.
    TreeNode* convertToBST(vector<int>& nums, int left, int right) {
        if (left > right) {  // Base case: No elements to process
            return nullptr;
        }
        
        int mid = left + (right - left) / 2;  // Find the middle element
        TreeNode* node = new TreeNode(nums[mid]);  // Create a new tree node for middle element
        
        // Recursively build left and right subtrees
        node->left = convertToBST(nums, left, mid - 1);  // Left subtree
        node->right = convertToBST(nums, mid + 1, right);  // Right subtree
        
        return node;
    }

    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return convertToBST(nums, 0, nums.size() - 1);  // Start the recursive process
    }
};
```

This approach efficiently balances the BST by selecting the middle element, ensuring the tree remains balanced, provided the input array is sorted.

