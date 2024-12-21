# [Leetcode 94: Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Approaches:
- [Approach 1: Recursive Inorder Traversal](#approach-1-recursive-inorder-traversal)
- [Approach 2: Iterative Inorder Traversal using Stack](#approach-2-iterative-inorder-traversal-using-stack)
- [Approach 3: Morris Inorder Traversal (Threaded Binary Tree)](#approach-3-morris-inorder-traversal-threaded-binary-tree)

### Approach 1: Recursive Inorder Traversal

**Intuition:**

The recursive approach to inorder traversal is straightforward. We follow the order: left subtree, root, and then right subtree. This can be naturally expressed using recursion as the same operations are applied to each subtree.

**Code:**
```cpp
class Solution {
public:
    void inorderTraversalHelper(TreeNode* root, vector<int>& result) {
        if (root == nullptr) return;  // Base case: if the node is empty
        inorderTraversalHelper(root->left, result);  // Traverse left subtree
        result.push_back(root->val);  // Visit the root
        inorderTraversalHelper(root->right, result);  // Traverse right subtree
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        inorderTraversalHelper(root, result);  // Start the recursive function
        return result;
    }
};
```
**Time Complexity:** O(n)  
**Space Complexity:** O(n) because of the recursion stack and the result vector.

### Approach 2: Iterative Inorder Traversal using Stack

**Intuition:**

Instead of using recursion that implicitly uses the call stack, we can simulate the process explicitly with our own stack. The idea is to traverse down to the leftmost node, pushing each node onto the stack as we go. Once we hit the end, we pop nodes from the stack, visit them, and then explore their right subtrees.

**Code:**
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> stack;
        TreeNode* current = root;
        
        while (current != nullptr || !stack.empty()) {
            // Reach the leftmost node of the current subtree
            while (current != nullptr) {
                stack.push(current);
                current = current->left;
            }
            
            // Current is nullptr here, means the left end is reached
            current = stack.top();  // Node to visit
            stack.pop();  // Pop from stack
            result.push_back(current->val);  // Visit the node
            
            // Explore the current node's right subtree
            current = current->right;
        }
        
        return result;
    }
};
```
**Time Complexity:** O(n)  
**Space Complexity:** O(n) in the worst case for the stack, but less than the recursion stack in practice.

### Approach 3: Morris Inorder Traversal (Threaded Binary Tree)

**Intuition:**

Morris traversal eliminates the need for a stack or recursion by threading the tree. For each node, link it to its predecessor, traverse left subtree, then sever the link afterward. This way, we achieve O(1) space complexity without modifying the tree structure permanently.

**Code:**
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        TreeNode* current = root;
        
        while (current != nullptr) {
            if (current->left == nullptr) {
                // No left subtree, visit the node and go right
                result.push_back(current->val);
                current = current->right;
            } else {
                // Find the inorder predecessor of current
                TreeNode* pre = current->left;
                while (pre->right != nullptr && pre->right != current) {
                    pre = pre->right;
                }
                
                if (pre->right == nullptr) {
                    // Establish thread for backtracking
                    pre->right = current;
                    current = current->left;
                } else {
                    // Thread exiting: remove the thread and visit the node
                    pre->right = nullptr;
                    result.push_back(current->val);
                    current = current->right;
                }
            }
        }
        
        return result;
    }
};
```
**Time Complexity:** O(n)  
**Space Complexity:** O(1) as we make use of no additional data structures aside from the output vector.

