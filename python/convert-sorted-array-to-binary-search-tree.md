# [Leetcode Problem 108: Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

## Solutions

- [Solution 1: Recursive Binary Search Approach](#solution-1-recursive-binary-search-approach)

## Solution 1: Recursive Binary Search Approach

### Intuition

The task involves converting a sorted array into a height-balanced Binary Search Tree (BST). A height-balanced BST is defined as a binary tree in which the depth of the two subtrees of every node never differs by more than 1. Since the array is sorted, the middle element will make a perfect root, ensuring minimal height difference between left and right subtrees.

The recursive approach works by:
1. Choosing the middle element of the current subarray as the root of the subtree.
2. Recursively constructing the left subtree from the left half of the current subarray.
3. Recursively constructing the right subtree from the right half of the current subarray.

### Algorithm Steps

1. Consider the whole array initially.
2. Recursively find the middle element of the subarray.
3. Create a tree node with this middle element.
4. Recursively apply the same procedure to the left sublist to create the left subtree.
5. Recursively apply the same procedure to the right sublist to create the right subtree.
6. Return the root node of the constructed BST.

### Code

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def sortedArrayToBST(nums):
    # Helper function to perform recursion
    def convertToBST(left, right):
        if left > right:
            return None

        # Find the middle element of the current range
        mid = (left + right) // 2

        # The mid element becomes the root
        node = TreeNode(nums[mid])

        # Recursively build left subtree
        node.left = convertToBST(left, mid - 1)

        # Recursively build right subtree
        node.right = convertToBST(mid + 1, right)

        return node

    # Begin recursion with the whole range of the array
    return convertToBST(0, len(nums) - 1)
```

### Complexity Analysis

- **Time Complexity**: O(n), where n is the number of elements in the sorted array. Each element is visited once to form the tree.
- **Space Complexity**: O(log n), due to the space used by the recursion stack (since the tree is height-balanced, the height is log n).

