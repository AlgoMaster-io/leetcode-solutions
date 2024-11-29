# 144. [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approach 1: Recursive Preorder Traversal

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
#include <vector>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        preorder(root, result);
        return result;
    }

private:
    void preorder(TreeNode* node, vector<int>& result) {
        if (node == nullptr) {
            return; // Base case: if the node is null, return
        }

        result.push_back(node->val); // Visit the root
        preorder(node->left, result); // Recurse for the left subtree
        preorder(node->right, result); // Recurse for the right subtree
    }
};
```

## Approach 2: Iterative Preorder Traversal Using Stack

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
#include <vector>
#include <stack>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) {
            return result; // Return empty result if the tree is empty
        }

        stack<TreeNode*> stack;
        stack.push(root);

        while (!stack.empty()) {
            TreeNode* currentNode = stack.top();
            stack.pop();
            result.push_back(currentNode->val); // Visit the root

            // Push right child first so that left child is processed first
            if (currentNode->right != nullptr) {
                stack.push(currentNode->right);
            }
            if (currentNode->left != nullptr) {
                stack.push(currentNode->left);
            }
        }

        return result;
    }
};
```

## Approach 3: Iterative Preorder Traversal Using Threaded Binary Tree

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
#include <vector>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        TreeNode* current = root;

        while (current != nullptr) {
            if (current->left == nullptr) {
                result.push_back(current->val); // Visit the root
                current = current->right; // Move to the right
            } else {
                TreeNode* predecessor = current->left;

                // Find the rightmost node in the left subtree
                while (predecessor->right != nullptr && predecessor->right != current) {
                    predecessor = predecessor->right;
                }

                if (predecessor->right == nullptr) {
                    result.push_back(current->val); // Visit the root
                    predecessor->right = current; // Create a temporary thread to the root
                    current = current->left; // Move to the left
                } else {
                    predecessor->right = nullptr; // Remove the temporary thread
                    current = current->right; // Move to the right
                }
            }
        }

        return result;
    }
};
```

