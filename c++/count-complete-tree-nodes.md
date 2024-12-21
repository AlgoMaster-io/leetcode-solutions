# [Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

## Solutions
- [Approach 1: Simple Tree Traversal](#approach-1-simple-tree-traversal)
- [Approach 2: Binary Search on Height](#approach-2-binary-search-on-height)
- [Approach 3: Complete Tree Characteristics](#approach-3-complete-tree-characteristics)

### Approach 1: Simple Tree Traversal

#### Intuition
The simplest way to count nodes in a binary tree is a basic traversal (either depth-first or breadth-first) and count each node encountered. Although this method is straightforward, it takes linear time as it examines each node.

#### C++ Code
```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (!root) {
            return 0; // Base case: if the root is null, return 0
        }
        
        // Recursively count the nodes in the left and right subtrees
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

#### Complexity
- **Time Complexity:** O(n), where n is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity:** O(h), where h is the height of the tree due to the recursion stack.

### Approach 2: Binary Search on Height

#### Intuition
In a complete binary tree, aside from the last level, all levels are completely filled. Thus, we can use binary search to efficiently determine the number of nodes in the last level. This approach reduces the complexity by focusing on the properties of complete trees.

1. Calculate the height of the tree.
2. Use binary search to count how many nodes exist in the last level.

#### C++ Code
```cpp
class Solution {
public:
    int computeTreeHeight(TreeNode* node) {
        int height = 0;
        while (node != nullptr) {
            height++; // Traverse down the leftmost edge
            node = node->left;
        }
        return height;
    }
    
    int countNodes(TreeNode* root) {
        if (!root) {
            return 0;
        }
        
        int leftHeight = computeTreeHeight(root->left);
        int rightHeight = computeTreeHeight(root->right);
        
        if (leftHeight == rightHeight) {
            // Left subtree is a perfect tree
            return (1 << leftHeight) + countNodes(root->right);
        } else {
            // Right subtree is a perfect tree
            return (1 << rightHeight) + countNodes(root->left);
        }
    }
};
```

#### Complexity
- **Time Complexity:** O((log n)^2), because computing tree height is O(log n) and we do this log(n) times.
- **Space Complexity:** O(h) due to recursion where h is the height of the tree.

### Approach 3: Complete Tree Characteristics

#### Intuition
Leveraging the complete binary tree properties further, one can identify the levels that are perfectly filled instantly. We can determine if the subtree is a perfect binary tree and use the formula for the number of nodes in a perfect binary tree (which is \(2^h - 1\)).

#### C++ Code
```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (!root) {
            return 0;
        }
        
        TreeNode* left = root->left;
        TreeNode* right = root->right;
        int leftHeight = 0, rightHeight = 0;
        
        // Calculate the left height
        while (left) {
            leftHeight++;
            left = left->left;
        }
        // Calculate the right height
        while (right) {
            rightHeight++;
            right = right->right;
        }
        
        if (leftHeight == rightHeight) {
            // It's a perfect tree
            return (1 << (leftHeight + 1)) - 1;
        }
        
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

#### Complexity
- **Time Complexity:** O(log^2 n)
- **Space Complexity:** O(log n) due to recursion stack.

This approach optimally leverages the complete tree property and reduces unnecessary traversal for counting nodes. It uses binary height measurements to decide quickly if part of the tree is fully populated.

