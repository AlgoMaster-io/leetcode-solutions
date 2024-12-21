# [Leetcode 530: Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

## Approaches
1. [In-order Traversal with Sorted Array](#approach-1)
2. [In-order Traversal with Constant Space Optimization](#approach-2)

---

## Approach 1: In-order Traversal with Sorted Array

### Intuition
A Binary Search Tree (BST) has the property where an in-order traversal will yield its elements in non-decreasing order. Therefore, if we perform an in-order traversal of the BST to collect all the elements, we can simply iterate through the sorted list and calculate the minimum difference between consecutive elements.

### Steps
1. Perform an in-order traversal of the BST and collect the node values in a vector.
2. Sort the vector (though in-order guarantees sorting).
3. Iterate through the sorted vector and track the minimum difference between consecutive values.

### Code
```cpp
#include <vector>
#include <algorithm>
#include <climits>
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
    void inOrderTraversal(TreeNode* root, vector<int>& vals) {
        if (!root) return;
        inOrderTraversal(root->left, vals);  // Visit left subtree
        vals.push_back(root->val);           // Visit current node
        inOrderTraversal(root->right, vals); // Visit right subtree
    }
    
    int getMinimumDifference(TreeNode* root) {
        if (!root) return 0;
        
        vector<int> vals;
        inOrderTraversal(root, vals);
        
        int minDiff = INT_MAX;
        for (int i = 1; i < vals.size(); ++i) {
            minDiff = min(minDiff, vals[i] - vals[i - 1]); // Check difference with previous element
        }
        
        return minDiff;
    }
};
```

### Complexity
- **Time Complexity**: O(n), where n is the number of nodes. We visit each node exactly once during the in-order traversal.
- **Space Complexity**: O(n) due to the additional storage in the vector for node values.

---

## Approach 2: In-order Traversal with Constant Space Optimization

### Intuition
We can optimize the space complexity by not storing all node values. Instead, we can keep track of the previous visited node value and calculate the difference on-the-fly during the in-order traversal.

### Steps
1. Perform an in-order traversal of the BST while maintaining a variable to track the previous node's value.
2. On visiting each node, compute the difference with the previous node's value.
3. Track the minimum difference found.

### Code
```cpp
#include <climits>
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
    int minDiff = INT_MAX;
    int prevVal = -1; // Use -1 as an indicator for the first node (can use any invalid/flag value)
    
    void inOrderTraversal(TreeNode* root) {
        if (!root) return;
        
        inOrderTraversal(root->left); // Visit left subtree
        
        if (prevVal != -1) { // Check if it's not the first visited node
            minDiff = min(minDiff, root->val - prevVal); // Calculate the difference
        }
        prevVal = root->val; // Update previous node value
        
        inOrderTraversal(root->right); // Visit right subtree
    }
    
    int getMinimumDifference(TreeNode* root) {
        inOrderTraversal(root);
        return minDiff;
    }
};
```

### Complexity
- **Time Complexity**: O(n), where n is the number of nodes. Each node is visited once.
- **Space Complexity**: O(h), where h is the height of the tree. This is the auxiliary stack space used for recursion. In the best case (balanced tree), \(h = \log n\); and in the worst case (skewed tree), \(h = n\).

