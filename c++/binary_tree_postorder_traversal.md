# 145. [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approach 1: Recursive Postorder Traversal

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
#include <vector>
using namespace std;

class TreeNode {
public:
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        postorder(root, result);
        return result;
    }

private:
    void postorder(TreeNode* node, vector<int>& result) {
        if (node == nullptr) {
            return; // Base case: if the node is null, return
        }

        postorder(node->left, result); // Recurse for the left subtree
        postorder(node->right, result); // Recurse for the right subtree
        result.push_back(node->val); // Visit the root
    }
};
```

## Approach 2: Iterative Postorder Traversal Using Two Stacks

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(2n) = O(n), for two stacks
#include <vector>
#include <stack>
using namespace std;

class TreeNode {
public:
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) {
            return result;
        }

        stack<TreeNode*> stack1, stack2;
        stack1.push(root);

        while (!stack1.empty()) {
            TreeNode* current = stack1.top();
            stack1.pop();
            stack2.push(current);

            if (current->left != nullptr) {
                stack1.push(current->left); // Push left child to stack1
            }
            if (current->right != nullptr) {
                stack1.push(current->right); // Push right child to stack1
            }
        }

        while (!stack2.empty()) {
            result.push_back(stack2.top()->val); // Add nodes in postorder from stack2
            stack2.pop();
        }

        return result;
    }
};
```

## Approach 3: Iterative Postorder Traversal Using One Stack

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
#include <vector>
#include <stack>
using namespace std;

class TreeNode {
public:
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) {
            return result;
        }

        stack<TreeNode*> stack;
        TreeNode* current = root;
        TreeNode* lastVisited = nullptr;

        while (current != nullptr || !stack.empty()) {
            if (current != nullptr) {
                stack.push(current); // Push nodes to the stack
                current = current->left; // Move to the left subtree
            } else {
                TreeNode* peekNode = stack.top();
                if (peekNode->right != nullptr && lastVisited != peekNode->right) {
                    current = peekNode->right; // Move to the right subtree
                } else {
                    result.push_back(peekNode->val); // Visit the node
                    lastVisited = stack.top();
                    stack.pop();
                }
            }
        }

        return result;
    }
};
```

## Approach 4: Morris Postorder Traversal (Space Optimized)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(1), as no additional space is used
#include <vector>
using namespace std;

class TreeNode {
public:
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        TreeNode dummy(0);
        dummy.left = root;
        TreeNode* current = &dummy, *predecessor = nullptr;

        while (current != nullptr) {
            if (current->left == nullptr) {
                current = current->right;
            } else {
                predecessor = current->left;
                while (predecessor->right != nullptr && predecessor->right != current) {
                    predecessor = predecessor->right;
                }

                if (predecessor->right == nullptr) {
                    predecessor->right = current; // Create a temporary thread
                    current = current->left;
                } else {
                    reverseAddPath(current->left, predecessor, result);
                    predecessor->right = nullptr; // Remove the temporary thread
                    current = current->right;
                }
            }
        }

        return result;
    }

private:
    void reverseAddPath(TreeNode* start, TreeNode* end, vector<int>& result) {
        vector<int> temp;
        while (start != end) {
            temp.push_back(start->val);
            start = start->right;
        }
        temp.push_back(end->val);
        for (int i = temp.size() - 1; i >= 0; i--) {
            result.push_back(temp[i]); // Add nodes in reverse order
        }
    }
};
```

