# [Leetcode 783: Minimum Distance Between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

---

## Approaches

1. [In-order Traversal with Array](#in-order-traversal-with-array)
2. [Optimized In-order Traversal](#optimized-in-order-traversal)

---

### In-order Traversal with Array

#### Intuition:

We can find the minimum distance between the values of any two nodes in a BST by leveraging the in-order traversal property. During the in-order traversal, we visit the nodes in a non-decreasing order. Thus, to find the smallest difference, storing the traversal order in an array will allow us to compute adjacent differences easily.

#### Steps:

1. Perform an in-order traversal of the BST and store the values in a sorted array.
2. Traverse the array to find the minimum difference between consecutive elements.

This approach takes two passes: one to generate the sorted values and one to compute the minimal difference.

#### Time Complexity:

- O(n) for in-order traversal and O(n) for one pass through the array, total: O(n)
- Space Complexity: O(n) for storing node values

```cpp
class Solution {
public:
    // Helper function for in-order traversal
    void inorderTraversal(TreeNode* node, vector<int>& values) {
        if (!node) return;
        inorderTraversal(node->left, values);           // Visit left subtree
        values.push_back(node->val);                   // Process current node
        inorderTraversal(node->right, values);         // Visit right subtree
    }

    int minDiffInBST(TreeNode* root) {
        vector<int> values;                            // Vector to store values in sorted order
        inorderTraversal(root, values);                // Fill the values array with in-order traversal

        int minDiff = INT_MAX;                         // Initialize minDiff with a large value
        for (int i = 1; i < values.size(); ++i) {
            minDiff = min(minDiff, values[i] - values[i - 1]); // Compute the min difference
        }
        
        return minDiff;                                // Return the smallest difference found
    }
};
```

---

### Optimized In-order Traversal

#### Intuition:

To further optimize space usage, we can compute the minimum difference directly during the in-order traversal without having to store all node values. By keeping track of the previous node visited, we can compare it to the current node during traversal and update the minimum difference accordingly.

#### Steps:

1. Perform an in-order traversal on the BST.
2. Maintain a previous node pointer and update the minimum difference during traversal.
3. Update the previous pointer as we move forward.

This eliminates the need for additional space to store node values.

#### Time Complexity:

- O(n) for in-order traversal.
- Space Complexity: O(h), where h is the height of the tree, which is the call stack for recursion.

```cpp
class Solution {
public:
    int minDiffInBST(TreeNode* root) {
        int minDiff = INT_MAX;                         // Initialize minDiff with a large maximum integer value
        int prevVal = -1;                              // Previous node value initialized to -1 (since all BST values are positive by constraint)

        // Helper lambda function for in-order traversal
        function<void(TreeNode*)> inorder = [&](TreeNode* node) {
            if (!node) return;                         // Base case: if node is null, return
            
            inorder(node->left);                       // Traverse left subtree
            if (prevVal != -1) {                       // Check if previous value is set
                minDiff = min(minDiff, node->val - prevVal); // Update minDiff with the current node value minus previous node value
            }
            prevVal = node->val;                       // Update previous value to current node's value
            inorder(node->right);                      // Traverse right subtree
        };

        inorder(root);                                 // Start the in-order traversal from root
        return minDiff;                                // Return the minimal difference found
    }
};
```

This solution leverages recursion efficiently, maintaining O(n) time complexity without the additional space overhead used for storing the entire array of values, leading to a more optimal and memory-efficient solution.

