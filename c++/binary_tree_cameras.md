# 968. [Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approach 1: Greedy Approach with Recursion

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(h) where h is the height of the tree (recursion stack)
class Solution {
private:
    int cameras = 0;

public:
    int minCameraCover(TreeNode* root) {
        return dfs(root) == 0 ? cameras + 1 : cameras;
    }

    // Helper function for recursive traversal
    int dfs(TreeNode* node) {
        if (node == nullptr) {
            return 1; // This node is covered
        }

        int left = dfs(node->left);
        int right = dfs(node->right);

        if (left == 0 || right == 0) {
            cameras++;
            return 2; // Placing a camera at this node
        }

        return (left == 2 || right == 2) ? 1 : 0;
        // 2 indicates child has a camera, 1 means this node is covered
        // 0 indicates this node needs a camera
    }
};

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

## Approach 2: Dynamic Programming with State Tracking

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_map>
#include <vector>

class Solution {
private:
    static constexpr int HAS_CAMERA = 0;
    static constexpr int COVERED = 1;
    static constexpr int NEEDS_CAMERA = 2;

public:
    int minCameraCover(TreeNode* root) {
        std::unordered_map<TreeNode*, std::vector<int>> dp;
        return dfs(root, dp) == NEEDS_CAMERA ? dp[root][HAS_CAMERA] + 1 : dp[root][HAS_CAMERA];
    }

    int dfs(TreeNode* node, std::unordered_map<TreeNode*, std::vector<int>>& dp) {
        if (node == nullptr) {
            return COVERED;
        }

        if (dp.find(node) == dp.end()) {
            std::vector<int> state(3, 0);

            int left = dfs(node->left, dp);
            int right = dfs(node->right, dp);

            if (left == NEEDS_CAMERA || right == NEEDS_CAMERA) {
                state[HAS_CAMERA] = dp.count(node) ? dp[node][HAS_CAMERA] + 1 : 1;
                dp[node] = state;
                return HAS_CAMERA;
            }

            if (left == HAS_CAMERA || right == HAS_CAMERA) {
                state[COVERED] = dp.count(node) ? dp[node][COVERED] : 0;
                dp[node] = state;
                return COVERED;
            }

            state[NEEDS_CAMERA] = dp.count(node) ? dp[node][NEEDS_CAMERA] : 0;
            dp[node] = state;
            return NEEDS_CAMERA;
        }

        return dp[node][0];
    }
};

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

Note: The second approach is a detailed version using dynamic programming concepts, but the greedy recursive version is typically more straightforward and efficient due to its optimizations for this specific problem.

