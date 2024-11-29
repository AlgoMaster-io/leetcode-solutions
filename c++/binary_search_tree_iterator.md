# 173. [Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approach 1: Controlled Recursion Using Stack

### Solution
```cpp
// Time Complexity:
//   - Constructor: O(h), where h is the height of the tree
//   - next(): O(1) on average across all calls
//   - hasNext(): O(1)
// Space Complexity: O(h), where h is the height of the tree for the stack

#include <stack>

class TreeNode {
public:
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class BSTIterator {
private:
    std::stack<TreeNode*> stk;

    void pushLeft(TreeNode* node) {
        while (node != nullptr) {
            stk.push(node);
            node = node->left;
        }
    }

public:
    BSTIterator(TreeNode* root) {
        pushLeft(root);
    }

    /** @return the next smallest number */
    int next() {
        TreeNode* node = stk.top();
        stk.pop();
        if (node->right != nullptr) {
            pushLeft(node->right);
        }
        return node->val;
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !stk.empty();
    }
};
```

## Approach 2: Precomputed Inorder Traversal

### Solution
```cpp
// Time Complexity:
//   - Constructor: O(n), where n is the number of nodes in the tree
//   - next(): O(1)
//   - hasNext(): O(1)
// Space Complexity: O(n), where n is the number of nodes in the tree

#include <vector>

class TreeNode {
public:
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class BSTIterator {
private:
    std::vector<int> inorder;
    int index;

    void inorderTraversal(TreeNode* node) {
        if (node == nullptr) {
            return;
        }
        inorderTraversal(node->left);
        inorder.push_back(node->val);
        inorderTraversal(node->right);
    }

public:
    BSTIterator(TreeNode* root) {
        index = 0;
        inorderTraversal(root);
    }

    /** @return the next smallest number */
    int next() {
        return inorder[index++];
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return index < inorder.size();
    }
};
```

## Approach 3: Morris Traversal (Space Optimized, Less Practical for Iterator)

### Solution
```cpp
// Time Complexity:
//   - Constructor: O(1) (no precomputation)
//   - next(): O(1) on average across all calls
//   - hasNext(): O(1)
// Space Complexity: O(1), no additional storage

class TreeNode {
public:
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class BSTIterator {
private:
    TreeNode* current;
    TreeNode* predecessor;

public:
    BSTIterator(TreeNode* root) {
        current = root;
    }

    /** @return the next smallest number */
    int next() {
        int value = -1;

        while (current != nullptr) {
            if (current->left == nullptr) {
                value = current->val;
                current = current->right;
                break;
            } else {
                predecessor = current->left;

                while (predecessor->right != nullptr && predecessor->right != current) {
                    predecessor = predecessor->right;
                }

                if (predecessor->right == nullptr) {
                    predecessor->right = current; // Create a temporary link
                    current = current->left;
                } else {
                    predecessor->right = nullptr; // Remove the temporary link
                    value = current->val;
                    current = current->right;
                    break;
                }
            }
        }

        return value;
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return current != nullptr;
    }
};
```

