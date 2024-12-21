# [Leetcode 230: Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

## Approaches

- [Approach 1: Brute Force with Inorder Traversal Array](#approach-1-brute-force-with-inorder-traversal-array)
- [Approach 2: Optimized Inorder Traversal with Counter](#approach-2-optimized-inorder-traversal-with-counter)

### Approach 1: Brute Force with Inorder Traversal Array

**Intuition:**

The simplest way to find the k-th smallest element in a BST is to perform an in-order traversal. The property of the in-order traversal for a BST is that it visits nodes in a non-decreasing order. Hence, the k-th visited node is the k-th smallest element.

**Steps:**

1. Perform an in-order traversal of the BST and store the elements in an array.
2. Return the k-th element from the array.

**Code:**

```cpp
class Solution {
public:
    // Helper function to perform inorder traversal and collect elements
    void inorderTraversal(TreeNode* root, vector<int>& elements) {
        if (root == nullptr) return;
        
        // Inorder traversal: Left -> Node -> Right
        inorderTraversal(root->left, elements);
        elements.push_back(root->val);  // Visiting the node
        inorderTraversal(root->right, elements);
    }
    
    int kthSmallest(TreeNode* root, int k) {
        vector<int> elements;
        inorderTraversal(root, elements);
        return elements[k - 1];  // return the k-th smallest element (0-based index)
    }
};
```

**Complexity Analysis:**

- **Time Complexity:** O(N), where N is the number of nodes in the tree, as we visit each node once.
- **Space Complexity:** O(N), for storing the elements in the traversal array.

### Approach 2: Optimized Inorder Traversal with Counter

**Intuition:**

Instead of storing all the nodes during the traversal, we can keep a counter and stop as soon as we reach the k-th element.

**Steps:**

1. Initiate a counter to keep track of the number of nodes visited during the Inorder traversal.
2. Perform an in-order traversal and increment the counter.
3. Once the counter equals k, return the current node's value.

**Code:**

```cpp
class Solution {
public:
    // Helper function for inorder traversal and tracking the k-th element
    int inorderTraversal(TreeNode* root, int& k) {
        if (root == nullptr) return -1;
        
        // LEFT: Dive into the left subtree
        int left = inorderTraversal(root->left, k);
        if (k == 0) return left;  // If we've found the k-th element in the left subtree, return it
        
        // NODE: Process the current node
        k--;  // Visiting the current node, decrement k
        if (k == 0) return root->val;  // If after decrementing, k is zero, this is the k-th element
        
        // RIGHT: Dive into the right subtree
        return inorderTraversal(root->right, k);
    }
    
    int kthSmallest(TreeNode* root, int k) {
        return inorderTraversal(root, k);
    }
};
```

**Complexity Analysis:**

- **Time Complexity:** O(H + k), where H is the height of the tree. In the worst case, for an unbalanced tree, it can go up to O(N).
- **Space Complexity:** O(H), due to the recursion stack, which depends on the height of the tree.

Both approaches effectively solve the problem, with the second approach being more space-efficient by avoiding the use of an auxiliary array to store elements.

