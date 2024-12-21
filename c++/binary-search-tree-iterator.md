# [Leetcode 173: Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

### Approaches:
- [Approach 1: In-Order Traversal Using Recursion](#approach-1-in-order-traversal-using-recursion)
- [Approach 2: In-Order Traversal Using Iteration and Stack](#approach-2-in-order-traversal-using-iteration-and-stack)

## Approach 1: In-Order Traversal Using Recursion

### Intuition:
The simplest way to solve this problem is to perform an in-order traversal of the tree and store the nodes in an array. In an in-order traversal of a BST, the elements are visited in a sorted manner. Thus, we can then iterate through this array one by one while implementing the iterator functions.

### Code:
```cpp
class BSTIterator {
private:
    vector<int> nodes;
    int index;
    
    // Helper function to perform in-order traversal and store the result in nodes
    void inorderTraversal(TreeNode* root) {
        if (!root) return;
        inorderTraversal(root->left);
        nodes.push_back(root->val);
        inorderTraversal(root->right);
    }
    
public:
    BSTIterator(TreeNode* root) {
        index = 0;
        inorderTraversal(root);
    }
    
    /** @return the next smallest number */
    int next() {
        return nodes[index++];
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return index < nodes.size();
    }
};
```

### Complexity Analysis:
- **Time Complexity:** O(N) for the constructor, O(1) for both `next()` and `hasNext()`, where N is the number of nodes in the tree.
- **Space Complexity:** O(N) due to the space required for the `nodes` vector.

### Pros and Cons:
- **Pros:** Simple and easy to implement.
- **Cons:** Uses extra space which is not optimal, especially if the tree is very large.

## Approach 2: In-Order Traversal Using Iteration and Stack

### Intuition:
Instead of storing all the nodes in advance, we can use a stack to simulate the recursive in-order traversal. This way, the space usage is optimized to O(h), where h is the height of the tree. Each call to `next()` will compute the next in-order node in the BST dynamically, which saves memory.

### Code:
```cpp
class BSTIterator {
private:
    stack<TreeNode*> nodeStack;
    
    // Push all the left children of the node onto the stack
    void pushLeft(TreeNode* root) {
        while (root) {
            nodeStack.push(root);
            root = root->left;
        }
    }
    
public:
    BSTIterator(TreeNode* root) {
        pushLeft(root);
    }
    
    /** @return the next smallest number */
    int next() {
        TreeNode* node = nodeStack.top();
        nodeStack.pop();
        if (node->right) {
            pushLeft(node->right);
        }
        return node->val;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !nodeStack.empty();
    }
};
```

### Complexity Analysis:
- **Time Complexity:** O(1) on average for both `next()` and `hasNext()`. Each node is processed once.
- **Space Complexity:** O(h), where h is the height of the tree, due to the stack.

### Pros and Cons:
- **Pros:** More space efficient than Approach 1 as it doesn't store all nodes beforehand.
- **Cons:** Slightly more complex than the recursive in-order traversal due to the stack management. However, it's still quite efficient and optimal.

Both approaches solve the problem effectively, but Approach 2 is preferred for handling larger trees since it only uses stack space proportionate to the height of the tree.

