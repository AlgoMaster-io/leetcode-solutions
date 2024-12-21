# [Leetcode 145: Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach using Two Stacks](#iterative-approach-using-two-stacks)
3. [Iterative Approach using One Stack](#iterative-approach-using-one-stack)

### Recursive Approach

The simplest way to approach the postorder traversal problem (Left, Right, Root) is by utilizing recursion. Recursion follows a pattern similar to a depth-first search by visiting all the nodes in the left subtree, then the right subtree, and finally the root node.

#### Intuition
The intuition behind recursion here is straightforward. We make a recursive call for the left subtree to process all nodes in it, then for the right subtree, and ultimately process the root node. This clear sequence naturally corresponds to the postorder traversal.

#### Steps:
1. Define a helper function that takes a node and the list `result` where the nodes' values are appended.
2. Recursively traverse the left subtree first.
3. Recursively traverse the right subtree.
4. Append the current root node's value to the result list.

#### Code

```cpp
class Solution {
public:
    void postorderTraversalHelper(TreeNode* node, vector<int>& result) {
        if (node == nullptr)
            return;
        // Traverse the left subtree
        postorderTraversalHelper(node->left, result);
        // Traverse the right subtree
        postorderTraversalHelper(node->right, result);
        // Visit the root node
        result.push_back(node->val);
    }
    
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        postorderTraversalHelper(root, result);
        return result;
    }
};
```

#### Time Complexity: 
- O(n), where n is the number of nodes in the binary tree, since each node is visited once.
  
#### Space Complexity:
- O(h), where h is the height of the tree. This space is used by the call stack due to recursion.

### Iterative Approach using Two Stacks

Unlike the recursive method, the iterative approach utilizes data structures explicitly to mimic the implicit stack usage in recursion. Here, two stacks help keep track of nodes yet to be processed and nodes in postorder sequence returned.

#### Intuition
The two-stack method simulates the recursive approach using iteration. The first stack is used to reverse the order of processing nodes – usually postorder is left-right-root, but this stack achieves a root-right-left order. Using the second stack, we reverse this order to transform it back into the desired postorder sequence.

#### Steps:
1. Push the root node into the first stack.
2. While the first stack is not empty:
   - Pop a node from the first stack, push it into the second stack.
   - Push the node's left child into the first stack.
   - Push the node's right child into the first stack.
3. The second stack now contains nodes in postorder position, pop all elements and append them to the result list.

#### Code

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) 
            return result; // Edge case: empty tree

        stack<TreeNode*> stack1, stack2;
        stack1.push(root);

        while (!stack1.empty()) {
            TreeNode* node = stack1.top();
            stack1.pop();
            stack2.push(node);

            // Push left and right children of popped node to first stack
            if (node->left)
                stack1.push(node->left);
            if (node->right)
                stack1.push(node->right);
        }
        
        // Pop all items from the second stack to get postorder sequence
        while (!stack2.empty()) {
            TreeNode* node = stack2.top();
            stack2.pop();
            result.push_back(node->val);
        }

        return result;
    }
};
```

#### Time Complexity: 
- O(n), as every node is processed twice. Inserting and removing nodes from the stacks are O(1) operations.
  
#### Space Complexity:
- O(n), used for the two stacks due to iterative traversal.

### Iterative Approach using One Stack

By carefully controlling the visiting order of nodes, it is possible to simulate postorder traversal with one stack, using an auxiliary tracking pointer.

#### Intuition
This approach leverages a stack for traversal, using a pointer to keep track of the last visited node. This helps in determining whether to visit the node or continue the traversal down a subtree.

#### Steps:
1. Initialize an empty stack and set the lastVisitedNode to nullptr.
2. While the stack is not empty or the current node is not null:
   - Traverse left down the tree and push every node to the stack.
   - Peek at the top node. If it doesn't have a right child or its right child is already processed:
     - Visit the node, pop it from the stack, and update the lastVisitedNode.
   - Otherwise, set the current node to the current node's right child.

#### Code

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> stk;
        TreeNode* lastVisitedNode = nullptr;
        
        while (!stk.empty() || root != nullptr) {
            if (root != nullptr) {
                // Traverse down the left subtree
                stk.push(root);
                root = root->left;
            } else {
                TreeNode* peekNode = stk.top();
                // Check if right child is unvisited
                if (peekNode->right != nullptr && lastVisitedNode != peekNode->right) {
                    root = peekNode->right;
                } else {
                    result.push_back(peekNode->val); // Visit the node
                    lastVisitedNode = stk.top();
                    stk.pop();
                }
            }
        }
        return result;
    }
};
```

#### Time Complexity: 
- O(n), every node gets pushed and popped from the stack once.

#### Space Complexity:
- O(h), where h is the maximum height of the tree used for the stack.

Given these approaches, each has its merits for certain constraints such as iterative depth, space usage, and simplicity.

