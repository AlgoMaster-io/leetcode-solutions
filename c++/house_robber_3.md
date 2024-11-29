# 337. [House Robber III](https://leetcode.com/problems/house-robber-iii/)

## Approach 1: Recursive Approach with Memoization

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_map>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
private:
    std::unordered_map<TreeNode*, int> memo;

public:
    int rob(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        if (memo.find(root) != memo.end()) {
            return memo[root];
        }

        // if we rob this root, we cannot rob its direct children
        int robRoot = root->val;
        if (root->left != nullptr) {
            robRoot += rob(root->left->left) + rob(root->left->right);
        }
        if (root->right != nullptr) {
            robRoot += rob(root->right->left) + rob(root->right->right);
        }

        // if we do not rob this root, we are free to rob its children
        int skipRoot = rob(root->left) + rob(root->right);

        // Choose the maximum money we can rob
        int result = std::max(robRoot, skipRoot);
        memo[root] = result; // Memoize the result for current node

        return result;
    }
};
```

## Approach 2: Dynamic Programming with Tree DP

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(h), where h is the height of the tree
#include <unordered_map>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    int rob(TreeNode* root) {
        int* result = robSub(root);
        int maxRob = std::max(result[0], result[1]);
        delete[] result;
        return maxRob;
    }

private:
    int* robSub(TreeNode* root) {
        if (root == nullptr) {
            return new int[2]{0, 0}; // Entry for {not rob, rob}
        }

        int* left = robSub(root->left);
        int* right = robSub(root->right);

        // if we don't rob this node, we can choose to rob or not to rob its children
        int notRob = std::max(left[0], left[1]) + std::max(right[0], right[1]);

        // if we rob this node, we cannot rob its children
        int rob = root->val + left[0] + right[0];

        delete[] left;
        delete[] right;

        return new int[2]{notRob, rob}; // Return array containing {not rob, rob}
    }
};
```

