# [Leetcode 124: Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approaches
- [Recursive Postorder Traversal with Global Maximum](#recursive-postorder-traversal)

## Solution

### Recursive Postorder Traversal with Global Maximum

**Intuition:**

The problem requires us to find the maximum path sum in a binary tree, where the path can start and end at any node. The key insight here is to use a recursive postorder traversal to explore each node's maximum path sum contribution, considering the maximum gain from its left and/or right child.

- For each node, we calculate the maximum gains from its left and right sub-trees.
- The contribution of a node to the path that includes it as an intermediate node can be calculated as `node.val + left_gain + right_gain`.
- The side path sum we take to the parent node should be `node.val + max(left_gain, right_gain)`, because we can only include one side of the node.
- We maintain a global maximum path sum to track the maximum path we encounter during traversal.

**Algorithm:**
1. Use a helper recursive function to traverse the tree in a postorder manner (left, right, node).
2. At each node, calculate the `left_gain` by exploring the left subtree and `right_gain` by exploring the right subtree.
3. Compute the potential maximum path including both left and right child, and update the global `maxSum` if this path is greater.
4. Return the maximum path sum including the node and one of its children to its parent.

```csharp
public class Solution {
    private int maxSum = int.MinValue; // Initialize global max to the smallest possible value

    public int MaxPathSum(TreeNode root) {
        MaxGain(root); // Start the recursive postorder traversal
        return maxSum; // The result is stored in maxSum
    }

    private int MaxGain(TreeNode node) {
        if (node == null) {
            return 0; // Base case: null nodes contribute 0 to the path sum
        }

        // Recursively obtain the maximum path sum from the left and right subtrees
        // Use Math.Max to avoid negative sums (a negative sum would decrease the overall path sum)
        int leftGain = Math.Max(MaxGain(node.left), 0);
        int rightGain = Math.Max(MaxGain(node.right), 0);

        // The price of Path through the current node
        int priceNewPath = node.val + leftGain + rightGain;

        // Update the global maxSum. This value considers the current node as the highest point
        maxSum = Math.Max(maxSum, priceNewPath);

        // Return the maximum gain the current node, and one subtree can contribute.
        return node.val + Math.Max(leftGain, rightGain);
    }
}
```

**Time Complexity**: \(O(N)\), where \(N\) is the number of nodes in the binary tree. Each node is visited once.

**Space Complexity**: \(O(H)\), where \(H\) is the height of the tree. This accounts for the space used by the recursion stack. In the worst case, \(H = N\) for a skewed tree, leading to \(O(N)\) space complexity.

