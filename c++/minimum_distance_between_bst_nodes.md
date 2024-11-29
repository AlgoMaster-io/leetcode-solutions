# 783. [Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

## Approach 1: Inorder Traversal with a List

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n) for storing the inorder traversal
#include <vector>
#include <climits>
using namespace std;

// Definition for a binary tree node.
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
    int minDiffInBST(TreeNode* root) {
        vector<int> values;
        inorder(root, values);

        int minDiff = INT_MAX;
        for (int i = 1; i < values.size(); i++) {
            minDiff = min(minDiff, values[i] - values[i - 1]);
        }

        return minDiff;
    }

private:
    void inorder(TreeNode* node, vector<int>& values) {
        if (node == nullptr) {
            return;
        }

        inorder(node->left, values); // Recurse for the left subtree
        values.push_back(node->val); // Add the current node value
        inorder(node->right, values); // Recurse for the right subtree
    }
};
```

## Approach 2: Inorder Traversal with Constant Space

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
#include <climits>

class Solution {
public:
    int minDiffInBST(TreeNode* root) {
        prev = nullptr;
        minDiff = INT_MAX;
        inorder(root);
        return minDiff;
    }

private:
    TreeNode* prev;
    int minDiff;

    void inorder(TreeNode* node) {
        if (node == nullptr) {
            return;
        }

        inorder(node->left); // Recurse for the left subtree

        if (prev != nullptr) {
            minDiff = min(minDiff, node->val - prev->val); // Calculate the difference with the previous value
        }
        prev = node; // Update the previous node value

        inorder(node->right); // Recurse for the right subtree
    }
};
```

## Approach 3: Morris Traversal (Space Optimized)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
#include <climits>

class Solution {
public:
    int minDiffInBST(TreeNode* root) {
        TreeNode* current = root;
        TreeNode* prev = nullptr;
        int minDiff = INT_MAX;

        while (current != nullptr) {
            if (current->left == nullptr) {
                // Process the current node
                if (prev != nullptr) {
                    minDiff = min(minDiff, current->val - prev->val);
                }
                prev = current;
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
                    if (prev != nullptr) {
                        minDiff = min(minDiff, current->val - prev->val);
                    }
                    prev = current;
                    current = current->right; // Move to the right subtree
                }
            }
        }

        return minDiff;
    }
};
```

