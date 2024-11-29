# 199. [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

## Approach 1: Breadth-First Search (Using Queue)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for the queue and result storage
#include <vector>
#include <queue>

using namespace std;

class TreeNode {
public:
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) {
            return result; // Return empty result if the tree is empty
        }

        queue<TreeNode*> queue;
        queue.push(root);

        while (!queue.empty()) {
            int levelSize = queue.size();
            TreeNode* lastNode = nullptr;

            for (int i = 0; i < levelSize; i++) {
                TreeNode* currentNode = queue.front();
                queue.pop(); // Process the current node
                lastNode = currentNode;

                if (currentNode->left != nullptr) {
                    queue.push(currentNode->left); // Add left child to the queue
                }
                if (currentNode->right != nullptr) {
                    queue.push(currentNode->right); // Add right child to the queue
                }
            }

            result.push_back(lastNode->val); // Add the last node's value from this level
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

class TreeNode {
public:
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        dfs(root, 0, result);
        return result;
    }

private:
    void dfs(TreeNode* node, int depth, vector<int>& result) {
        if (node == nullptr) {
            return; // Base case: if the node is null, return
        }

        if (depth == result.size()) {
            result.push_back(node->val); // Add the first node of this depth (rightmost node)
        }

        dfs(node->right, depth + 1, result); // Visit the right subtree first
        dfs(node->left, depth + 1, result); // Visit the left subtree next
    }
};
```

