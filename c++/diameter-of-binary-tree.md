# [Leetcode 543: Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

## Approaches:
1. [Recursive DFS with Calculating Height](#approach-1)
2. [Optimized Recursive DFS with One Pass](#approach-2)

---

## Approach 1: Recursive DFS with Calculating Height

### Intuition:
The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root. One straightforward way to solve the problem is to calculate the height of each subtree and use these heights to find the diameter. 

### Steps:
1. Define a recursive function to calculate the height of a subtree. This function should also update the maximum diameter found so far.
2. For any node, the maximum path goes through the node is the sum of the heights of the left and right subtrees.
3. Use a helper variable to track the maximum diameter as we traverse the tree.

### C++ Code:
```cpp
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
    int diameterOfBinaryTree(TreeNode* root) {
        int diameter = 0;
        height(root, diameter);
        return diameter;
    }
    
private:
    int height(TreeNode* node, int& diameter) {
        if (!node) return 0;

        // Recursively find the height of left and right subtrees
        int leftHeight = height(node->left, diameter);
        int rightHeight = height(node->right, diameter);
        
        // Update the diameter if the current path is larger
        diameter = std::max(diameter, leftHeight + rightHeight);
        
        // Return the height of the current subtree
        return 1 + std::max(leftHeight, rightHeight);
    }
};
```

### Time Complexity:
- **O(n)**, where `n` is the number of nodes in the tree, as each node is visited once.

### Space Complexity:
- **O(h)**, where `h` is the height of the tree due to the recursive stack.

## Approach 2: Optimized Recursive DFS with One Pass

### Intuition:
In the previous approach, the height and diameter are calculated in a single pass, but each recursive call uses a helper function to find both. This can be optimized by pairing the return values of height and diameter within the same function, eliminating the need for an externally maintained maximum variable.

### Steps:
1. Modify the recursive function to return both the height of the node and its diameter.
2. At each step, compute both the height and update the maximum diameter simultaneously.

### C++ Code:
```cpp
class Solution2 {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        return heightAndDiameter(root).second;
    }

private:
    // Pair stores {height, diameter}
    std::pair<int, int> heightAndDiameter(TreeNode* node) {
        if (!node) return {0, 0};  // Base case: height and diameter are zero

        // Recursively find the height and diameter of left and right subtrees
        auto left = heightAndDiameter(node->left);
        auto right = heightAndDiameter(node->right);
        
        // Calculate current height
        int currentHeight = 1 + std::max(left.first, right.first);
        
        // Calculate maximum diameter at current node
        int currentDiameter = std::max({left.second, right.second, left.first + right.first});

        // Return a pair {Height, Diameter}
        return {currentHeight, currentDiameter};
    }
};
```

### Time Complexity:
- **O(n)**, as we visit each node once, computing height and diameter simultaneously.

### Space Complexity:
- **O(h)**, due to recursion stack space, where `h` is the height of the tree. 

This optimized approach simplifies understanding by combining calculations into a single recursive traversal.

