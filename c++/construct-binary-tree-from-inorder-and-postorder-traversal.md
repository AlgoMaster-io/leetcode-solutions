# [Leetcode 106: Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

## Approaches
1. [Recursive Approach using Hash Map for Indexing](#Approach-1)
2. [Iterative Approach using Stack](#Approach-2)

### Approach 1: Recursive Approach using Hash Map for Indexing

#### Intuition
The binary tree can be constructed from its inorder and postorder traversal. The postorder traversal's last element is the tree's root. Once we have the root, we can identify which elements belong to the left subtree and which belong to the right subtree using the inorder array. By using a hash map to store the indices of elements in the inorder array, we can efficiently find these elements, allowing us to construct the binary tree recursively.

#### Steps
- Obtain the root from the last element of the postorder array.
- Find the index of this root in the inorder array using a hash map.
- Elements to the left in the inorder array belong to the left subtree, and elements to the right belong to the right subtree.
- Recursively repeat the above steps for left and right partitions to construct the entire tree.

#### Code
```cpp
#include <unordered_map>
#include <vector>
#include <iostream>

using namespace std;

// Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        unordered_map<int, int> inorderIndex;
        for (int i = 0; i < inorder.size(); i++) {
            inorderIndex[inorder[i]] = i; // Map value to index for efficient lookup
        }
        return createTree(postorder, 0, postorder.size() - 1, inorder, 0, inorder.size() - 1, inorderIndex);
    }
    
private:
    TreeNode* createTree(vector<int>& postorder, int postStart, int postEnd, 
                         vector<int>& inorder, int inStart, int inEnd, 
                         unordered_map<int, int>& inorderIndex) {
        if (postStart > postEnd || inStart > inEnd) return nullptr; // Base case

        int rootVal = postorder[postEnd]; // The current root value
        TreeNode* root = new TreeNode(rootVal);
        int rootIndex = inorderIndex[rootVal]; // Index of root in inorder array
        
        // Calculate the number of elements in the left subtree
        int leftTreeCount = rootIndex - inStart;

        // Recursively build the left and right subtrees
        root->left = createTree(postorder, postStart, postStart + leftTreeCount - 1, 
                                inorder, inStart, rootIndex - 1, inorderIndex);
        root->right = createTree(postorder, postStart + leftTreeCount, postEnd - 1, 
                                 inorder, rootIndex + 1, inEnd, inorderIndex);

        return root; // Return the constructed tree
    }
};
```

#### Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the tree.
- **Space Complexity**: O(n) for the recursion stack and hash map.

### Approach 2: Iterative Approach using Stack

#### Intuition
In this approach, we simulate the recursive process of building the binary tree using a stack to track nodes. By iterating from the end of the postorder array, we progressively build the tree by arranging the nodes in the order they appear in postorder.

#### Steps
- Start from the last element in postorder, which is the root.
- Use a stack to manage the construction of the tree.
- As we build, each node is checked against the inorder array to determine if its right or left child should be processed.
- Use a stack to simulate the recursive behavior non-recursively.

#### Code
```cpp
#include <stack>

class Solution2 {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (postorder.empty()) return nullptr;

        stack<TreeNode*> s;
        TreeNode* root = new TreeNode(postorder.back()); // Root of the tree
        s.push(root);
        postorder.pop_back();

        while (!postorder.empty()) {
            TreeNode* node = new TreeNode(postorder.back());
            postorder.pop_back();

            if (!s.empty() && s.top()->val != inorder.back()) {
                s.top()->right = node; // Attach as right child
            } else {
                TreeNode* parent = nullptr;
                while (!s.empty() && s.top()->val == inorder.back()) {
                    parent = s.top();
                    s.pop();
                    inorder.pop_back();
                }
                parent->left = node; // Attach as left child
            }

            s.push(node); // Push the current node onto the stack
        }

        return root;
    }
};
```

#### Complexity
- **Time Complexity**: O(n), where n is the number of nodes in the tree.
- **Space Complexity**: O(n) due to the stack usage.

By carefully choosing and implementing the algorithm according to the use-case requirements, these approaches solve the problem of constructing a binary tree from inorder and postorder traversal efficiently.

