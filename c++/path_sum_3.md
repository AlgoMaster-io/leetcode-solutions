# 437. [Path Sum III](https://leetcode.com/problems/path-sum-iii/)

## Approach 1: Brute Force (DFS for Each Node)

### Solution
```cpp
// Time Complexity: O(n^2) in the worst case (skewed tree), O(n log n) for balanced tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
#include <cstddef>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};

class Solution {
public:
    int pathSum(TreeNode* root, int targetSum) {
        if (root == nullptr) {
            return 0; // Base case: empty tree
        }

        // Calculate paths including the current root and recurse for left and right subtrees
        return dfs(root, targetSum) + pathSum(root->left, targetSum) + pathSum(root->right, targetSum);
    }

private:
    int dfs(TreeNode* node, int targetSum) {
        if (node == nullptr) {
            return 0; // Base case: empty node
        }

        int count = 0;
        if (node->val == targetSum) {
            count++; // Path ends at the current node
        }

        // Check paths including left and right children
        count += dfs(node->left, targetSum - node->val);
        count += dfs(node->right, targetSum - node->val);

        return count;
    }
};
```

## Approach 2: Optimized Using Prefix Sum (HashMap)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h) for recursion stack and O(n) for unordered_map storage
#include <unordered_map>
#include <cstddef>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};

class Solution {
public:
    int pathSum(TreeNode* root, int targetSum) {
        std::unordered_map<int, int> prefixSumMap;
        prefixSumMap[0] = 1; // Base case: one way to get sum 0
        return dfs(root, 0, targetSum, prefixSumMap);
    }

private:
    int dfs(TreeNode* node, int currentSum, int targetSum, std::unordered_map<int, int>& prefixSumMap) {
        if (node == nullptr) {
            return 0; // Base case: empty node
        }

        currentSum += node->val;
        int paths = prefixSumMap[currentSum - targetSum]; // Count paths ending at this node

        // Update the prefix sum map for this node
        prefixSumMap[currentSum]++;

        // Recurse for left and right children
        paths += dfs(node->left, currentSum, targetSum, prefixSumMap);
        paths += dfs(node->right, currentSum, targetSum, prefixSumMap);

        // Backtrack: remove the current node's contribution to the prefix sum
        prefixSumMap[currentSum]--;

        return paths;
    }
};
```

