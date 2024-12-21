# [LeetCode 105: Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## Approaches
- [Recursive Approach](#recursive-approach)

---

## Recursive Approach

### Intuition:
The problem revolves around using the characteristics of preorder and inorder traversals to construct the binary tree. Let's break down the process:

- **Preorder traversal** gives nodes in the order of:
  - Root
  - Left subtree
  - Right subtree

- **Inorder traversal** provides:
  - Left subtree
  - Root
  - Right subtree

Given that in preorder, the first element is always the root, we can use it as a pivotal point to divide the inorder list into left and right subtrees. Subsequently, we just need to recursively construct these subtrees using the derived segments of both lists.

### Detailed Recursive Process:
1. **Base Case**: If either the preorder or inorder list is empty, we return `nullptr`.
  
2. **Recursive Case**:
   - Identify the root node from the beginning of the `preorder` list.
   - Find the index of this root in the `inorder` list. This index splits the `inorder` list into left and right subtrees.
   - Recursively create the left subtree using the elements in `preorder` and `inorder` that correspond to the left subtree.
   - Recursively create the right subtree using the elements that belong to the right subtree.

### Code:
```cpp
#include <unordered_map>
#include <vector>
using namespace std;

// Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        // Map to store inorder index for every node value
        unordered_map<int, int> inorderMap;
        for (int i = 0; i < inorder.size(); ++i) {
            inorderMap[inorder[i]] = i;
        }
        // Initiate recursive tree construction
        return buildSubTree(preorder, 0, 0, inorder.size() - 1, inorderMap);
    }
    
private:
    // Recursive function to construct the binary tree
    TreeNode* buildSubTree(vector<int>& preorder, int preStart, int inStart, int inEnd, unordered_map<int, int>& inorderMap) {
        if (preStart >= preorder.size() || inStart > inEnd) {
            return nullptr;
        }
        
        // The first element in preorder is the root
        int rootVal = preorder[preStart];
        TreeNode* root = new TreeNode(rootVal);
        
        // Find the root in inorder to separate left and right subtrees
        int inRootIndex = inorderMap[rootVal];
        
        // Elements left to the root in inorder are part of left subtree
        // Recursively build left subtree
        root->left = buildSubTree(preorder, preStart + 1, inStart, inRootIndex - 1, inorderMap);
        
        // Elements right to the root in inorder are part of right subtree
        // Calculate number of nodes in the left subtree to get the correct preorder start index for right subtree
        int leftTreeSize = inRootIndex - inStart;
        root->right = buildSubTree(preorder, preStart + leftTreeSize + 1, inRootIndex + 1, inEnd, inorderMap);
        
        return root;
    }
};
```

### Time Complexity:
- **O(n)**: We traverse each node once, and using a hashmap ensures that finding the index in the inorder list is O(1).

### Space Complexity:
- **O(n)**: Storing the inorder index mapping takes O(n) space, plus O(n) calls on the recursion stack for the worst-case unbalanced tree.

