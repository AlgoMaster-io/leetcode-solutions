# [LeetCode 124: Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Approaches:
- [Recursive Approach with Divide and Conquer](#recursive-approach-with-divide-and-conquer)

### Recursive Approach with Divide and Conquer

**Intuition:**

In this problem, we need to find a path where the sum of node values is maximized in a binary tree. A path could start and end at any node. A crucial observation here is that the maximum path can be computed using a postorder traversal because a node's value can contribute to the global maximum path only after we have computed the maximum paths of its left and right children.

By using a recursive function, we can compute the maximum path sum that can pass through each node, and at each node, we maintain a maximum sum that could be either:
1. Left sub-tree maximum path + node's value
2. Right sub-tree maximum path + node's value
3. Both left and right sub-tree maximums + node's value
4. Only the node's value itself.

The result of this recursive call needs to be updated at every step where we find a larger path sum. This is because the path doesn’t have to pass through the root, but can end at any node in the sub-tree.

**Algorithm Steps:**
1. Define a recursive function `max_gain(node)` which computes the maximum path sum through the node, which can be included in one of its parent's paths.
2. Use a global variable `max_sum` to track the maximum path sum encountered so far.
3. For each node, compute the maximum gain from its left and right child recursively.
4. Consider the four scenarios and update the `max_sum` with the maximum of these.
5. Return the maximum gain including the current node and one of its children.

**C++ Implementation:**

```cpp
#include <iostream>
#include <algorithm>
#include <climits>

// Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    int maxPathSum(TreeNode* root) {
        int max_sum = INT_MIN; // Initialize to lowest to handle negative values
        max_gain(root, max_sum); 
        return max_sum; // Return the updated maximum path sum
    }

private:
    int max_gain(TreeNode* node, int& max_sum) {
        if (!node) return 0; // Base case

        // Recursively calculate the maximum gain from left and right subtrees
        int left_gain = std::max(max_gain(node->left, max_sum), 0); // Consider only non-negative gains
        int right_gain = std::max(max_gain(node->right, max_sum), 0);

        // Current node's maximum path sum including both subtrees
        int current_max_path = node->val + left_gain + right_gain;

        // Update the global maximum path sum with the current node's maximum path sum
        max_sum = std::max(max_sum, current_max_path);

        // Return the maximum gain to be used by parent node (either including left or right child)
        return node->val + std::max(left_gain, right_gain);
    }
};
```
**Time Complexity:** O(N), where N is the number of nodes. We visit each node once.

**Space Complexity:** O(H), where H is the height of the tree. This is the space used by the call stack during recursion in the worst case (skewed tree scenario).

This solution leverages depth-first search, efficiently passing the maximum gains up the tree while keeping track of the highest path sum encountered using a global variable. This approach divides the problem at each node and conquers it by combining the solutions from subtrees, making it an optimal solution for this problem.

