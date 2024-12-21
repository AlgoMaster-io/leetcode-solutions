# [Leetcode 98: Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

## Solutions

1. [Approach 1: Recursive Inorder Traversal](#approach-1)
2. [Approach 2: Iterative Inorder Traversal](#approach-2)
3. [Approach 3: Recursive Range Check](#approach-3)

### Approach 1: Recursive Inorder Traversal

In a Binary Search Tree (BST), an inorder traversal should yield the values of the nodes in ascending order. We can utilize this property to validate if a tree is a BST. The idea is to perform an inorder traversal on the tree and ensure that the values encountered are in a strictly increasing order. 

**Steps:**
1. Perform an inorder traversal of the tree.
2. While traversing, compare each value with the previously traversed value to make sure it is greater.
3. If we ever find a current value that is not greater than the previous value, the tree is not a BST.

**Time Complexity:** O(N), where N is the number of nodes in the tree, because each node is visited once.

**Space Complexity:** O(N) for the space used by the recursion stack.

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return inorder(root);
    }
    
private:
    TreeNode* prev = nullptr;
    
    bool inorder(TreeNode* node) {
        if (!node) return true;
        
        // Traverse the left subtree
        if (!inorder(node->left)) return false;
        
        // Check the current node with the previous node
        if (prev && node->val <= prev->val) return false;
        prev = node; // Update prev to current node
        
        // Traverse the right subtree
        return inorder(node->right);
    }
};
```

### Approach 2: Iterative Inorder Traversal

We can use an iterative approach with a stack to perform inorder traversal. This eliminates the recursion stack overhead.

**Steps:**
1. Use a stack to simulate the recursive function call stack.
2. Traverse the left subtree, then the current node, followed by the right subtree.
3. During the traversal, compare each node's value with the last processed value to ensure it is greater.

**Time Complexity:** O(N), where N is the number of nodes in the tree.

**Space Complexity:** O(N), for the stack that holds nodes during traversal.

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (!root) return true;

        std::stack<TreeNode*> stack;
        TreeNode* current = root;
        TreeNode* prev = nullptr;

        while (current || !stack.empty()) {
            // Reach the leftmost node of the current node
            while (current) {
                stack.push(current);
                current = current->left;
            }
            // Current must be null at this point
            current = stack.top();
            stack.pop();
            // Check if the previous value is greater or equal to the current value
            if (prev && current->val <= prev->val) return false;
            prev = current; // update prev to current
            // We have visited the node and its left subtree, now it's right subtree's turn
            current = current->right;
        }
        return true;
    }
};
```

### Approach 3: Recursive Range Check

A more intuitive approach is to use the properties of BST: every node must follow the property where all values left to it are smaller and all values to the right are greater. We check these properties recursively for each node. 

For each node:
- The left subtree should have all values less than the node’s value.
- The right subtree should have all values greater than the node’s value.

**Steps:**
1. Use recursion with parameters passing down the range constraints.
2. Initially, the range can be set to negative/positive infinity.
3. If any node value does not fit the constraints, return false.

**Time Complexity:** O(N), where N is the number of nodes in the tree.

**Space Complexity:** O(N) for the recursion stack.

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return validate(root, nullptr, nullptr);
    }
    
    bool validate(TreeNode* node, TreeNode* minNode, TreeNode* maxNode) {
        if (!node) return true;
        
        // Ensure the current node's value is in the valid range
        if ((minNode && node->val <= minNode->val) || (maxNode && node->val >= maxNode->val))
            return false;
        
        // Check the left subtree with updated max constraint
        // Check the right subtree with updated min constraint
        return validate(node->left, minNode, node) && validate(node->right, node, maxNode);
    }
};
```

Each of these solutions progressively optimizes memory usage and conceptual implementation, providing different trade-offs that are useful depending on constraints and design preferences.

