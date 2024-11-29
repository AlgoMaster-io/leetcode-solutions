# 543. [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

## Approach 1: Depth-First Search (DFS) with Recursive Depth Calculation

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
class Solution {
public:
    int diameter = 0;

    int diameterOfBinaryTree(TreeNode* root) {
        depth(root);
        return diameter;
    }

private:
    int depth(TreeNode* node) {
        if (node == nullptr) {
            return 0; // Base case: null node has depth 0
        }

        int leftDepth = depth(node->left);  // Calculate depth of left subtree
        int rightDepth = depth(node->right); // Calculate depth of right subtree

        // Update the diameter (maximum path length between two nodes)
        diameter = std::max(diameter, leftDepth + rightDepth);

        // Return the depth of the current subtree
        return std::max(leftDepth, rightDepth) + 1;
    }
};
```

## Approach 2: Bottom-Up DFS with Global Variable (Optimized)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack
class Solution {
public:
    int maxDiameter = 0;

    int diameterOfBinaryTree(TreeNode* root) {
        calculateDepth(root);
        return maxDiameter;
    }

private:
    int calculateDepth(TreeNode* node) {
        if (node == nullptr) {
            return 0; // Null nodes contribute 0 to depth
        }

        int left = calculateDepth(node->left);  // Left subtree depth
        int right = calculateDepth(node->right); // Right subtree depth

        maxDiameter = std::max(maxDiameter, left + right); // Update the max diameter

        return std::max(left, right) + 1; // Return the current depth
    }
};
```

## Approach 3: Iterative DFS Using Stack

### Solution
```cpp
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for the stack
#include <stack>
#include <unordered_map>

class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        if (!root) {
            return 0; // Edge case: empty tree
        }

        std::stack<TreeNode*> stack;
        std::unordered_map<TreeNode*, int> depthMap;
        int maxDiameter = 0;

        stack.push(root);
        while (!stack.empty()) {
            TreeNode* current = stack.top();
            if (current == nullptr) {
                stack.pop();
                continue;
            }

            if ((current->left && depthMap.find(current->left) == depthMap.end()) || 
                (current->right && depthMap.find(current->right) == depthMap.end())) {
                if (current->left) stack.push(current->left);
                if (current->right) stack.push(current->right);
            } else {
                stack.pop();
                int left = depthMap[current->left];
                int right = depthMap[current->right];
                maxDiameter = std::max(maxDiameter, left + right);
                depthMap[current] = std::max(left, right) + 1;
            }
        }

        return maxDiameter;
    }
};
```

