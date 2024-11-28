# Convert Sorted Array to Binary Search Tree
Given an integer array nums where the elements are sorted in ascending order, convert it to a height-balanced Binary Search Tree (BST).

A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than 1.

### Constraints:
- 1 <= nums.length <= 10^4
- -10^4 <= nums[i] <= 10^4
- nums is sorted in a strictly increasing order.

### Examples
```javascript
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted as a correct answer.

Input: nums = [1,3]
Output: [3,1]
```

## Approaches to Solve the Problem
### Approach 1: Recursive Division (Optimal Solution)
##### Intuition:
To create a height-balanced BST from a sorted array, the key is to choose the middle element of the array as the root of the tree. This way, the left half of the array becomes the left subtree, and the right half becomes the right subtree, ensuring the tree remains balanced.

- The middle element of the array is always the root of the current subtree.
- Recursively divide the left half and the right half of the array to create the left and right subtrees, respectively.

Steps:
1. Base Case: If the array is empty, return None.
2. Recursive Case:
   - Find the middle element of the current array slice, which will be the root of the subtree.
   - Recursively build the left subtree using the left half of the array.
   - Recursively build the right subtree using the right half of the array.
3. Return the root node.
##### Visualization:
For nums = [-10, -3, 0, 5, 9], the recursion works as follows:

```rust
Initial Array: [-10, -3, 0, 5, 9]
Root: 0 (middle of the array)

Left Subtree: [-10, -3]
    Root: -3
    Left Subtree: [-10] → Root: -10
    Right Subtree: [] → None

Right Subtree: [5, 9]
    Root: 9
    Left Subtree: [5] → Root: 5
    Right Subtree: [] → None

The resulting BST will look like this:
        0
       / \
     -3   9
     /   /
   -10  5
```
##### Time Complexity:
O(n), where n is the number of elements in the array. Each element is processed once to create a node.
##### Space Complexity:
O(log n), due to the recursion depth. The space complexity is proportional to the height of the tree, which is O(log n) for a balanced BST.
##### Python Code:
```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def sortedArrayToBST(nums):
    # Base case: If array is empty, return None
    if not nums:
        return None
    
    # Recursive case: Pick the middle element as the root
    mid = len(nums) // 2
    root = TreeNode(nums[mid])
    
    # Recursively build the left and right subtrees
    root.left = sortedArrayToBST(nums[:mid])
    root.right = sortedArrayToBST(nums[mid+1:])
    
    return root
```
##### Explanation of Code: 
1. Base Case: The base case for the recursion is when the input array nums is empty ([]). In that case, we return None, which means no node is created.
2. Recursive Case: We pick the middle element of the array (using integer division len(nums) // 2) to be the root of the current subtree. Then, we recursively call the function on the left and right halves of the array to build the left and right subtrees.
3. TreeNode Construction: The TreeNode class is used to create nodes for the BST, with the value of each node being the middle element of the current subarray.
##### Edge Cases
1. Single Element: If the input array has only one element, it will become the root of the tree, and both left and right children will be None.
2. Empty Array: If the input array is empty, the function should return None, representing an empty tree.
3. Array with Two Elements: For an array with two elements, the middle element (or the first element, if there is no exact middle) will be the root, and the second element will be its right child.
### Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Recursive Division (Optimal)		             | O(n)      | O(log n)             |

The Recursive Division approach is the most efficient way to solve this problem. The time complexity is linear, as every element is processed once, and the space complexity is logarithmic due to the recursive nature of the algorithm.