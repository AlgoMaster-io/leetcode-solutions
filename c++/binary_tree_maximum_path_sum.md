# 124. [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approach: Depth-First Search (DFS)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
class Solution {
private:
    int maxSum = INT_MIN;

public:
    int maxPathSum(TreeNode* root) {
        calculateMaxPath(root);
        return maxSum;
    }

private:
    int calculateMaxPath(TreeNode* node) {
        if (node == nullptr) {
            return 0; // Base case: null node contributes 0 to the path sum
        }

        // Recursively calculate the maximum path sum of the left and right subtrees
        int leftMax = std::max(0, calculateMaxPath(node->left));  // Ignore negative sums
        int rightMax = std::max(0, calculateMaxPath(node->right)); // Ignore negative sums

        // Update the global maximum path sum
        maxSum = std::max(maxSum, leftMax + rightMax + node->val);

        // Return the maximum path sum including the current node and one of its subtrees
        return std::max(leftMax, rightMax) + node->val;
    }
};
```

