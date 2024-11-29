# 98. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Approach 1: Inorder Traversal (Iterative)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
#include <stack>

class Solution {
public:
    struct TreeNode {
        int val;
        TreeNode* left;
        TreeNode* right;
        TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    };

    bool isValidBST(TreeNode* root) {
        std::stack<TreeNode*> stack;
        TreeNode* current = root;
        TreeNode* prev = nullptr;

        while (current != nullptr || !stack.empty()) {
            while (current != nullptr) {
                stack.push(current); // Push nodes of the left subtree
                current = current->left;
            }

            current = stack.top(); stack.pop(); // Process the top node

            if (prev != nullptr && current->val <= prev->val) {
                return false; // If the current value is not greater than the previous, it's invalid
            }
            prev = current;

            current = current->right; // Move to the right subtree
        }

        return true;
    }
};
```

## Approach 2: Recursive Inorder Traversal

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
class Solution {
public:
    struct TreeNode {
        int val;
        TreeNode* left;
        TreeNode* right;
        TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    };

    TreeNode* prev = nullptr;

    bool isValidBST(TreeNode* root) {
        return inorder(root);
    }

private:
    bool inorder(TreeNode* node) {
        if (node == nullptr) {
            return true; // Base case: empty tree is valid
        }

        if (!inorder(node->left)) {
            return false; // Left subtree is invalid
        }

        if (prev != nullptr && node->val <= prev->val) {
            return false; // Current value is not greater than the previous
        }
        prev = node; // Update the previous node

        return inorder(node->right); // Recurse for the right subtree
    }
};
```

## Approach 3: DFS with Range Validation

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
class Solution {
public:
    struct TreeNode {
        int val;
        TreeNode* left;
        TreeNode* right;
        TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    };

    bool isValidBST(TreeNode* root) {
        return validate(root, nullptr, nullptr);
    }

private:
    bool validate(TreeNode* node, TreeNode* lower, TreeNode* upper) {
        if (node == nullptr) {
            return true; // Base case: empty tree is valid
        }

        if ((lower != nullptr && node->val <= lower->val) || 
            (upper != nullptr && node->val >= upper->val)) {
            return false; // Node value violates the range constraints
        }

        // Recursively validate the left and right subtrees
        return validate(node->left, lower, node) && validate(node->right, node, upper);
    }
};
```

