# [Leetcode 669: Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Iterative Approach using Stack](#iterative-approach-using-stack)

## Recursive Approach

### Intuition
The task is to trim a binary search tree (BST) such that the resulting BST contains only the values between a given range `[low, high]`, inclusive. In a BST, for any given node, all values in its left subtree are smaller and all values in its right subtree are greater. We can leverage this property to efficiently trim the tree by recursively examining each node and adjusting its children.

1. If the current node's value is less than `low`, it means that the entire left subtree is out of range and can be discarded. Thus, we can move to the right subtree.
2. If the node's value is greater than `high`, the entire right subtree is out of range, so move to the left subtree.
3. If the node's value is within the range `[low, high]`, recursively trim both left and right subtrees.

### Solution

```cpp
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        // Base case: If the current node is null, return null
        if (!root) return nullptr;

        // If the value of the current node is less than low, move to the right subtree
        if (root->val < low) {
            return trimBST(root->right, low, high);
        }
        
        // If the value of the current node is greater than high, move to the left subtree
        if (root->val > high) {
            return trimBST(root->left, low, high);
        }

        // The current node is within range, so trim its left and right subtrees
        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
        
        // Return the current node as it is within range
        return root;
    }
};
```

#### Time Complexity
- O(n), where `n` is the number of nodes in the tree. In the worst case, we may visit all nodes of the tree.

#### Space Complexity
- O(n), which considers the space used by the recursion stack in the worst-case scenario, which is when the tree is unbalanced.

## Iterative Approach using Stack

### Intuition
An iterative approach can also be implemented using a stack, which simulates the function call stack used in recursion. We will use a loop to process each node similarly based on its value relative to the range `[low, high]`. By iteratively adjusting pointers instead of recursive calls, we avoid deep recursion which might be limited by stack size.

1. Use a stack to traverse the tree iteratively.
2. Apply the same logic as the recursive approach, but instead of recursive calls, use a stack to navigate nodes.
3. Adjust node pointers to exclude nodes that fall outside the given range.

### Solution

```cpp
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (!root) return nullptr;

        // Find the root to start with that's within the range, which becomes our new root
        while (root && (root->val < low || root->val > high)) {
            if (root->val < low) {
                root = root->right;
            } else if (root->val > high) {
                root = root->left;
            }
        }

        // Initialize a stack and begin an iterative DFS
        std::stack<TreeNode*> stk;
        if (root) stk.push(root);

        while (!stk.empty()) {
            TreeNode* node = stk.top();
            stk.pop();

            // Trim the left children
            while (node->left && node->left->val < low) {
                node->left = node->left->right;
            }
            if (node->left) stk.push(node->left);

            // Trim the right children
            while (node->right && node->right->val > high) {
                node->right = node->right->left;
            }
            if (node->right) stk.push(node->right);
        }

        return root;
    }
};
```

#### Time Complexity
- O(n), where `n` is the number of nodes in the tree, as each node is processed once.

#### Space Complexity
- O(n), mainly for the stack which can, in the worst-case scenario, hold all nodes of the tree.

In both the recursive and iterative approaches, nodes outside the `[low, high]` range are effectively pruned, maintaining the structure and properties of a BST for the remaining nodes.

