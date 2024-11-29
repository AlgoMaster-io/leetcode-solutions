# 103. [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Approach 1: Breadth-First Search with Direction Toggle

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage
#include <vector>
#include <deque>
#include <queue>

using namespace std;

// Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> result;
        if (!root) {
            return result; // Return empty result if the tree is empty
        }

        queue<TreeNode*> q;
        q.push(root);
        bool leftToRight = true;

        while (!q.empty()) {
            int levelSize = q.size();
            deque<int> currentLevel;

            for (int i = 0; i < levelSize; i++) {
                TreeNode* currentNode = q.front(); // Process the current node
                q.pop();

                if (leftToRight) {
                    currentLevel.push_back(currentNode->val); // Add at the end
                } else {
                    currentLevel.push_front(currentNode->val); // Add at the beginning
                }

                if (currentNode->left != nullptr) {
                    q.push(currentNode->left); // Add left child to the queue
                }
                if (currentNode->right != nullptr) {
                    q.push(currentNode->right); // Add right child to the queue
                }
            }

            result.push_back(vector<int>(currentLevel.begin(), currentLevel.end())); // Add the current level to the result
            leftToRight = !leftToRight; // Toggle the direction for the next level
        }

        return result;
    }
};
```

## Approach 2: Depth-First Search (Recursive with Level Tracking)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
#include <vector>
#include <deque>

using namespace std;

// Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> result;
        dfs(root, 0, result);
        return result;
    }

private:
    void dfs(TreeNode* node, int level, vector<vector<int>>& result) {
        if (!node) {
            return; // Base case: if the node is null, return
        }

        if (level == result.size()) {
            result.push_back(deque<int>()); // Create a new level in the result
        }

        if (level % 2 == 0) {
            result[level].push_back(node->val); // Add normally for even levels
        } else {
            result[level].insert(result[level].begin(), node->val); // Add to the front for odd levels
        }

        dfs(node->left, level + 1, result); // Recurse for the left child
        dfs(node->right, level + 1, result); // Recurse for the right child
    }
};
```

