# [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Approach 1: Recursive Construction

### Solution
csharp
```csharp
// Time Complexity: O(n^2) in the worst case (unbalanced tree) and O(n log n) on average
// Space Complexity: O(n) (due to recursion stack)
public class Solution {
    public TreeNode ConstructMaximumBinaryTree(int[] nums) {
        return BuildTree(nums, 0, nums.Length - 1);
    }

    private TreeNode BuildTree(int[] nums, int left, int right) {
        if (left > right) {
            return null; // Base case: no elements in the range
        }

        // Find the index of the maximum element in the current range
        int maxIndex = left;
        for (int i = left + 1; i <= right; i++) {
            if (nums[i] > nums[maxIndex]) {
                maxIndex = i;
            }
        }

        // Create the root node with the maximum value
        TreeNode root = new TreeNode(nums[maxIndex]);

        // Recursively construct the left and right subtrees
        root.left = BuildTree(nums, left, maxIndex - 1);
        root.right = BuildTree(nums, maxIndex + 1, right);

        return root;
    }
}
```

## Approach 2: Monotonic Stack (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public TreeNode ConstructMaximumBinaryTree(int[] nums) {
        Stack<TreeNode> stack = new Stack<TreeNode>();

        foreach (int num in nums) {
            TreeNode current = new TreeNode(num);

            // Maintain the stack such that the current node is the parent
            // of the last node in the stack if it has a smaller value
            while (stack.Count > 0 && stack.Peek().val < num) {
                current.left = stack.Pop();
            }

            // The last node in the stack becomes the right child of the current node
            if (stack.Count > 0) {
                stack.Peek().right = current;
            }

            // Push the current node onto the stack
            stack.Push(current);
        }

        // The root of the tree is the bottom-most node in the stack
        return stack.Count > 0 ? stack.ToArray()[0] : null;
    }
}
```

