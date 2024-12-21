# [Leetcode 144: Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Iterative Approach using Stack](#iterative-approach-using-stack)

---

## Recursive Approach

### Intuition
The recursive approach leverages the natural structure of binary trees for recursion. Preorder traversal follows the process of visiting the Root, Left, and Right subtree (R-L-R). Recursion naturally suits this traversal because you process each node and then recursively process its left subtree followed by its right subtree.

### Approach
1. Start with the root node and visit (or process) it.
2. Recursively traverse the left subtree.
3. Recursively traverse the right subtree.

### C++ Code

```cpp
#include <vector>

// Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    // Helper function to perform preorder traversal recursively
    void preorderTraversalHelper(TreeNode* node, std::vector<int>& result) {
        if (node == nullptr)
            return;

        // Step 1: Visit the root node
        result.push_back(node->val);
        // Step 2: Recursively traverse the left subtree
        preorderTraversalHelper(node->left, result);
        // Step 3: Recursively traverse the right subtree
        preorderTraversalHelper(node->right, result);
    }

    std::vector<int> preorderTraversal(TreeNode* root) {
        std::vector<int> result;
        preorderTraversalHelper(root, result);
        return result;
    }
};
```

### Time Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is visited exactly once.

### Space Complexity
- **Space Complexity**: O(h), where h is the height of the tree. This space is used for the recursion stack. In the worst case, it can grow to O(n) for a skewed tree.

---

## Iterative Approach using Stack

### Intuition
The iterative approach eliminates the recursion stack by using a stack data structure explicitly. By iterating with a stack, we can manually simulate the traversal order (Root, Left, Right) that would naturally happen in a recursive call.

### Approach
1. Initialize a stack and push the root node onto it.
2. While the stack is not empty:
   - Pop a node from the stack, visit it (process it), and push its value to the result vector.
   - Push the right child to the stack if it exists (this preserves preorder traversal).
   - Push the left child to the stack if it exists.

### C++ Code

```cpp
#include <vector>
#include <stack>

class Solution {
public:
    std::vector<int> preorderTraversal(TreeNode* root) {
        std::vector<int> result;
        if (root == nullptr)
            return result;

        std::stack<TreeNode*> nodeStack;
        nodeStack.push(root);

        while (!nodeStack.empty()) {
            TreeNode* node = nodeStack.top();
            nodeStack.pop();
            
            // Visit the node
            result.push_back(node->val);

            // Push the right child first so the left child is processed first
            if (node->right) {
                nodeStack.push(node->right);
            }
            if (node->left) {
                nodeStack.push(node->left);
            }
        }

        return result;
    }
};
```

### Time Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is visited exactly once.

### Space Complexity
- **Space Complexity**: O(h), where h is the height of the tree. In the worst case (a completely unbalanced tree), the space can become O(n).

