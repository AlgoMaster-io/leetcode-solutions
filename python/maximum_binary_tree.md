# [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Approach 1: Recursive Construction

### Solution

```python
# Time Complexity: O(n^2) in the worst case (unbalanced tree) and O(n log n) on average
# Space Complexity: O(n) (due to recursion stack)
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def constructMaximumBinaryTree(self, nums):
        return self.buildTree(nums, 0, len(nums) - 1)

    def buildTree(self, nums, left, right):
        if left > right:
            return None  # Base case: no elements in the range

        # Find the index of the maximum element in the current range
        maxIndex = left
        for i in range(left + 1, right + 1):
            if nums[i] > nums[maxIndex]:
                maxIndex = i

        # Create the root node with the maximum value
        root = TreeNode(nums[maxIndex])

        # Recursively construct the left and right subtrees
        root.left = self.buildTree(nums, left, maxIndex - 1)
        root.right = self.buildTree(nums, maxIndex + 1, right)

        return root
```

## Approach 2: Monotonic Stack (Optimal)

### Solution

```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def constructMaximumBinaryTree(self, nums):
        stack = []

        for num in nums:
            current = TreeNode(num)

            # Maintain the stack such that the current node is the parent
            # of the last node in the stack if it has a smaller value
            while stack and stack[-1].val < num:
                current.left = stack.pop()

            # The last node in the stack becomes the right child of the current node
            if stack:
                stack[-1].right = current

            # Push the current node onto the stack
            stack.append(current)

        # The root of the tree is the bottom-most node in the stack
        return stack[0]
```

