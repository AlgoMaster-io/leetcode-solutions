# [Leetcode 654: Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach Using Stack](#iterative-approach-using-stack)

---

### Recursive Approach

The most intuitive way to solve this problem is by using recursion. Given an array, the maximum binary tree can be constructed by following these steps:

1. Find the maximum element in the array. This will be the root of the tree.
2. The elements to the left of the maximum element will form the left subtree, and the elements to the right will form the right subtree.
3. Recursively apply the above steps to each subtree.

**Detailed Steps**:
- Find the index of the maximum element.
- Create a root node using the maximum element.
- Split the array into left and right subarrays and recursively build the left and right subtrees.

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        // Base case: if the array is empty, return null
        if (nums.empty()) return nullptr;
        
        // Find the index of the maximum element
        int maxIndex = max_element(nums.begin(), nums.end()) - nums.begin();
        
        // Create a tree node with the maximum element
        TreeNode* root = new TreeNode(nums[maxIndex]);
        
        // Construct the left subtree from elements before the maximum element
        vector<int> left(nums.begin(), nums.begin() + maxIndex);
        root->left = constructMaximumBinaryTree(left);
        
        // Construct the right subtree from elements after the maximum element
        vector<int> right(nums.begin() + maxIndex + 1, nums.end());
        root->right = constructMaximumBinaryTree(right);
        
        return root;
    }
};
```

**Time Complexity**: O(n^2) - We find the maximum element for each subtree, which takes O(n), and we have n such calls.

**Space Complexity**: O(n) - due to the recursive stack.

---

### Iterative Approach Using Stack

We can optimize the problem using a stack to build the tree in one pass.

**Intuition**:
- As we proceed through the list of numbers, we maintain a sequence of nodes in a stack.
- Each new number should replace the nodes in the stack that it is greater than because it will be the root for those nodes.
- Maintain the node that replaces the stack top as its left child.
- The new node becomes the right child of the last popped node.

**Detailed Steps**:
- Use a stack to keep track of TreeNodes.
- For each number, create a new TreeNode.
- While the stack is not empty and the top node's value is less than the current number, pop nodes from the stack.
- Set the last popped node as the left child of the new node.
- If the stack is not empty, set the new node as the right child of the current top node in the stack.
- Push the new node onto the stack.

```cpp
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        stack<TreeNode*> st;
        
        for (int num : nums) {
            TreeNode* curr = new TreeNode(num);
            // Pop from stack while the current number is greater than the stack's top node
            while (!st.empty() && st.top()->val < num) {
                // The top of the stack becomes left child to the current number
                curr->left = st.top();
                st.pop();
            }
            // If stack still has elements, current number becomes that top's right child
            if (!st.empty()) {
                st.top()->right = curr;
            }
            // Push the current number node onto the stack
            st.push(curr);
        }
        
        // The bottom of the stack is the root of the maximum binary tree
        TreeNode* root = nullptr;
        while (!st.empty()) {
            root = st.top();
            st.pop();
        }
        
        return root;
    }
};
```

**Time Complexity**: O(n) - Each element is pushed and popped at most once.

**Space Complexity**: O(n) - for the stack storage.

