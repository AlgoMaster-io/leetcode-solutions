# 94. [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approach 1: Recursive Inorder Traversal

### Solution
cpp
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
#include <vector>

using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        inorder(root, result);
        return result;
    }

private:
    void inorder(TreeNode* node, vector<int>& result) {
        if (node == nullptr) {
            return; // Base case: if the node is null, return
        }

        inorder(node->left, result); // Recurse for the left subtree
        result.push_back(node->val); // Visit the root
        inorder(node->right, result); // Recurse for the right subtree
    }
};
```

## Approach 2: Iterative Inorder Traversal Using Stack

### Solution
cpp
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
#include <vector>
#include <stack>

using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> stack;
        TreeNode* current = root;

        while (current != nullptr || !stack.empty()) {
            while (current != nullptr) {
                stack.push(current); // Push the current node and move to the left subtree
                current = current->left;
            }

            current = stack.top();
            stack.pop(); // Process the top node
            result.push_back(current->val); // Visit the node
            current = current->right; // Move to the right subtree
        }

        return result;
    }
};
```

## Approach 3: Morris Inorder Traversal (Without Recursion or Stack)

### Solution
cpp
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
#include <vector>

using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        TreeNode* current = root;

        while (current != nullptr) {
            if (current->left == nullptr) {
                result.push_back(current->val); // Visit the node
                current = current->right; // Move to the right subtree
            } else {
                TreeNode* predecessor = current->left;

                // Find the rightmost node in the left subtree
                while (predecessor->right != nullptr && predecessor->right != current) {
                    predecessor = predecessor->right;
                }

                if (predecessor->right == nullptr) {
                    predecessor->right = current; // Create a temporary thread to the root
                    current = current->left; // Move to the left subtree
                } else {
                    predecessor->right = nullptr; // Remove the temporary thread
                    result.push_back(current->val); // Visit the node
                    current = current->right; // Move to the right subtree
                }
            }
        }

        return result;
    }
};
```

