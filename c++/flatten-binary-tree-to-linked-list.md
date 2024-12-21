# [Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

## Navigation
- [Approach 1: Preorder Traversal with Extra Space](#approach-1-preorder-traversal-with-extra-space)
- [Approach 2: In-place Modification Using Right-pointer (Optimal)](#approach-2-in-place-modification-using-right-pointer-optimal)

## Approach 1: Preorder Traversal with Extra Space

### Intuition
The problem requires us to convert a binary tree into a flattened linked list where the right pointers mimic the preorder traversal of the tree. A straightforward approach is to perform a preorder traversal and store the nodes in a list, then rearrange the pointers to achieve the desired linked list format. This approach is intuitive and easy to implement but uses additional space to store nodes during traversal.

### Steps
1. Perform a preorder traversal of the binary tree and store the nodes in a list.
2. Iterate over the list of stored nodes, setting the `right` pointer of each node to the next node in the list and the `left` pointer to `nullptr`.
3. The reconstruction of the tree uses the preorder list and connects all nodes in the "right" fashion.

### Code
```cpp
#include <vector>

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
    void preorderTraversal(TreeNode* node, std::vector<TreeNode*>& nodeList) {
        if (!node) return;
        nodeList.push_back(node); // Store current node in list
        preorderTraversal(node->left, nodeList); // Traverse left subtree
        preorderTraversal(node->right, nodeList); // Traverse right subtree
    }
    
    void flatten(TreeNode* root) {
        if (!root) return;
        std::vector<TreeNode*> nodeList;
        preorderTraversal(root, nodeList);
        
        for (int i = 0; i < nodeList.size() - 1; ++i) {
            nodeList[i]->left = nullptr; // Clear left child
            nodeList[i]->right = nodeList[i + 1]; // Set right child to next node
        }
        nodeList.back()->left = nullptr;
        nodeList.back()->right = nullptr; // Last node
    }
};
```

### Complexity
- **Time Complexity**: \(O(n)\), where \(n\) is the number of nodes in the tree, as each node is visited exactly once during the traversal.
- **Space Complexity**: \(O(n)\) due to storing nodes in a list.

## Approach 2: In-place Modification Using Right-pointer (Optimal)

### Intuition
To flatten the tree without using additional space, we use a technique similar to a Morris traversal. The key idea is to press each left subtree into the right subtree using preorder traversal order, right in-place modification.

### Steps
1. Traverse the tree using a pointer `curr`.
2. For each node, if there is a left child, find the rightmost node in the left subtree (this node should be connected to the current node's right child).
3. Make the right pointer of the rightmost node point to the current's node right child.
4. Move the left child to be the right child, and set the left child to `nullptr`.
5. Move the `curr` pointer to the right to continue to the next node.
6. Repeat the above process until the entire tree is flattened.

### Code
```cpp
class Solution {
public:
    void flatten(TreeNode* root) {
        TreeNode* curr = root;
        
        while (curr) {
            if (curr->left) { // If there is a left subtree
                TreeNode* rightmost = curr->left;
                
                // Find the rightmost node of the left subtree
                while (rightmost->right) {
                    rightmost = rightmost->right;
                }
                
                // Connect the rightmost node to the current node's right child
                rightmost->right = curr->right;
                
                // Move the left subtree to the right and nullify the left child
                curr->right = curr->left;
                curr->left = nullptr;
            }
            
            // Move on to the right node
            curr = curr->right;
        }
    }
};
```

### Complexity
- **Time Complexity**: \(O(n)\), where \(n\) is the number of nodes in the tree. Each node is visited and processed at most a few times.
- **Space Complexity**: \(O(1)\), as we accomplish the tree flattening in place without extra list or stack.

