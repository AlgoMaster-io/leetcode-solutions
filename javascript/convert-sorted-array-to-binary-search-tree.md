# [Leetcode 108: Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

## Approaches
- [Recursive Approach](#recursive-approach)

## Recursive Approach

### Intuition
The problem requires us to convert a sorted array into a height-balanced Binary Search Tree (BST). A height-balanced BST is defined as a binary tree in which the depth of the two subtrees of every node never differs by more than 1.

The optimal way to achieve this is using recursion. Given the sorted nature of the array, the middle element of any sub-segment can be chosen as the root to ensure balance. This divides the segment into two halves which can be recursively processed to form left and right subtrees.

### Steps
1. **Base Case**: If the current subarray is empty (`left > right`), return `null`.
2. **Recursive Case**: Find the middle of the current subarray. This middle element becomes the root node for the current subtree.
3. Recursively determine the left child as the root of the subtree formed by the left subarray (`left` to `mid - 1`).
4. Recursively determine the right child as the root of the subtree formed by the right subarray (`mid + 1` to `right`).
5. Return the constructed subtree for each recursion.

### Code

```javascript
// Definition for a binary tree node.
function TreeNode(val) {
    this.val = val;
    this.left = this.right = null;
}

/**
 * @param {number[]} nums
 * @return {TreeNode}
 */
var sortedArrayToBST = function(nums) {
    // Helper recursive function to construct BST from nums
    function constructBST(left, right) {
        // Base case: when the left index exceeds the right,
        // it indicates an empty subarray
        if (left > right) {
            return null;
        }

        // Always choose the middle element to maintain balance
        const mid = Math.floor((left + right) / 2);
        
        // Create current node from the middle element
        const node = new TreeNode(nums[mid]);

        // Recursively construct the left subtree with elements
        // left of the middle element
        node.left = constructBST(left, mid - 1);

        // Recursively construct the right subtree with elements
        // right of the middle element
        node.right = constructBST(mid + 1, right);

        return node;
    }
    
    // Initiate the recursion from the full range of the array
    return constructBST(0, nums.length - 1);
};
```

### Time and Space Complexity

- **Time Complexity**: O(n), where n is the number of nodes in the BST. We visit each node exactly once.
- **Space Complexity**: O(log n), due to the recursion stack during the formation of a balanced BST. The maximum depth of recursion is log n corresponding to the height of the balanced tree.

This approach efficiently constructs a balanced BST from the given sorted array using a recursive strategy, leveraging the properties of the array structure to ensure balance in the tree.

