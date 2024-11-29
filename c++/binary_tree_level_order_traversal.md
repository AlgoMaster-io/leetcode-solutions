# 102. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## Approach 1: Breadth-First Search (Using Queue)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage
#include <vector>
#include <queue>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode* left, TreeNode* right) : val(x), left(left), right(right) {}
};

class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        if (root == nullptr) {
            return result; // Return empty result if the tree is empty
        }

        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int levelSize = q.size();
            vector<int> currentLevel;

            for (int i = 0; i < levelSize; i++) {
                TreeNode* currentNode = q.front();
                q.pop(); // Process the current node
                currentLevel.push_back(currentNode->val);

                if (currentNode->left != nullptr) {
                    q.push(currentNode->left); // Add left child to the queue
                }
                if (currentNode->right != nullptr) {
                    q.push(currentNode->right); // Add right child to the queue
                }
            }

            result.push_back(currentLevel); // Add the current level to the result
        }

        return result;
    }
};
```

## Approach 2: Depth-First Search (Recursive)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
#include <vector>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode* left, TreeNode* right) : val(x), left(left), right(right) {}
};

class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        dfs(root, 0, result);
        return result;
    }
    
private:
    void dfs(TreeNode* node, int level, vector<vector<int>>& result) {
        if (node == nullptr) {
            return; // Base case: if the node is null, return
        }

        if (level == result.size()) {
            result.push_back(vector<int>()); // Create a new level in the result
        }

        result[level].push_back(node->val); // Add the current node's value to the current level

        dfs(node->left, level + 1, result); // Recurse for the left child
        dfs(node->right, level + 1, result); // Recurse for the right child
    }
};
```

