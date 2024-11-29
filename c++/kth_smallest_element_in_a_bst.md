# 230. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

## Approach 1: Inorder Traversal (Recursive)

### Solution
c++
// Time Complexity: O(n), where n is the number of nodes in the tree (in the worst case)
// Space Complexity: O(h), where h is the height of the tree for the recursion stack
```cpp
class Solution {
private:
    int count = 0;
    int result = 0;

    void inorder(TreeNode* node, int k) {
        if (node == nullptr) {
            return;
        }

        inorder(node->left, k); // Recurse for the left subtree

        count++; // Increment the count for each visited node
        if (count == k) {
            result = node->val; // Store the kth smallest value
            return;
        }

        inorder(node->right, k); // Recurse for the right subtree
    }

public:
    int kthSmallest(TreeNode* root, int k) {
        inorder(root, k);
        return result;
    }
};
```

## Approach 2: Inorder Traversal (Iterative)

### Solution
c++
// Time Complexity: O(k), where k is the kth smallest element to be found
// Space Complexity: O(h), where h is the height of the tree for the stack
```cpp
#include <stack>

class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        std::stack<TreeNode*> stack;
        TreeNode* current = root;
        int count = 0;

        while (current != nullptr || !stack.empty()) {
            while (current != nullptr) {
                stack.push(current); // Push nodes of the left subtree
                current = current->left;
            }

            current = stack.top();
            stack.pop(); // Process the top node
            count++;

            if (count == k) {
                return current->val; // Return the kth smallest value
            }

            current = current->right; // Move to the right subtree
        }

        return -1; // This line should never be reached if k is valid
    }
};
```

## Approach 3: Binary Search on BST Property

### Solution
c++
// Time Complexity: O(h + k), where h is the height of the tree and k is the position of the kth smallest element
// Space Complexity: O(h), where h is the height of the tree for recursion stack
```cpp
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        int leftNodeCount = countNodes(root->left);

        if (leftNodeCount == k - 1) {
            return root->val; // Root is the kth smallest element
        } else if (leftNodeCount >= k) {
            return kthSmallest(root->left, k); // Recurse on the left subtree
        } else {
            return kthSmallest(root->right, k - leftNodeCount - 1); // Recurse on the right subtree
        }
    }

private:
    int countNodes(TreeNode* node) {
        if (node == nullptr) {
            return 0;
        }
        return 1 + countNodes(node->left) + countNodes(node->right); // Count nodes in the subtree
    }
};
```

