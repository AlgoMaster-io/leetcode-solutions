# [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Approach 1: Recursive Construction

### Solution
```java
// Time Complexity: O(n^2) in the worst case (unbalanced tree) and O(n log n) on average
// Space Complexity: O(n) (due to recursion stack)
public class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return buildTree(nums, 0, nums.length - 1);
    }

    private TreeNode buildTree(int[] nums, int left, int right) {
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
        root.left = buildTree(nums, left, maxIndex - 1);
        root.right = buildTree(nums, maxIndex + 1, right);

        return root;
    }
}
```

## Approach 2: Monotonic Stack (Optimal)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.Stack;

public class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        Stack<TreeNode> stack = new Stack<>();

        for (int num : nums) {
            TreeNode current = new TreeNode(num);

            // Maintain the stack such that the current node is the parent
            // of the last node in the stack if it has a smaller value
            while (!stack.isEmpty() && stack.peek().val < num) {
                current.left = stack.pop();
            }

            // The last node in the stack becomes the right child of the current node
            if (!stack.isEmpty()) {
                stack.peek().right = current;
            }

            // Push the current node onto the stack
            stack.push(current);
        }

        // The root of the tree is the bottom-most node in the stack
        return stack.firstElement();
    }
}
```