# [Leetcode 337: House Robber III](https://leetcode.com/problems/house-robber-iii/)

## Approaches:
1. [Recursive Traversal with Memoization](#recursive-traversal-with-memoization)
2. [Optimized DFS with State Tracking](#optimized-dfs-with-state-tracking)

## Approach 1: Recursive Traversal with Memoization

### Intuition:
The problem is based on choosing either a node or going a level deeper (child nodes) without breaking the no-adjacent node rule. At each step, we decide whether to include the current node’s value in our total or not. We use memoization to store already solved subproblems to avoid redundant calculations.

1. **Recursive Definition**:
   - If we rob the current node, we cannot rob its immediate children. Hence, add the current node’s value to the total and then jump to the child nodes' children.
   - If we do not rob the current node, we are free to rob or not rob its immediate children.

2. **Memoization**:
   - We can use a hashmap to store the results of each node to avoid calculating them multiple times.

```cpp
class TreeNode {
public:
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
    std::unordered_map<TreeNode*, int> memo;
    
    int robHelper(TreeNode* root) {
        if (!root) return 0;
        if (memo.find(root) != memo.end()) return memo[root];
        
        int robCurrent = root->val;
        if (root->left) {
            robCurrent += robHelper(root->left->left) + robHelper(root->left->right);
        }
        if (root->right) {
            robCurrent += robHelper(root->right->left) + robHelper(root->right->right);
        }
        
        int skipCurrent = robHelper(root->left) + robHelper(root->right);
        
        int result = std::max(robCurrent, skipCurrent);
        memo[root] = result;
        return result;
    }
    
public:
    int rob(TreeNode* root) {
        return robHelper(root);
    }
};
```

### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is processed once.
- **Space Complexity**: O(n), due to the recursion stack and hashmap storing n nodes.

## Approach 2: Optimized DFS with State Tracking

### Intuition:
We can further optimize the recursive solution by returning two values for each node:
- The maximum amount we can rob including the current node.
- The maximum amount we can rob excluding the current node.

This reduces the space needed to store results for the children since we do the calculations in a single pass.

1. **DFS with Tuple Results**:
   - Traverse the tree in a bottom-up manner.
   - For each node, compute two values as explained.

2. **Result Selection**:
   - At the end, select the better option between robbing including or excluding the root node.

```cpp
class Solution {
    std::pair<int, int> robSub(TreeNode* node) {
        if (!node) return {0, 0};
        
        auto left = robSub(node->left);
        auto right = robSub(node->right);
        
        int robCurrent = node->val + left.second + right.second;
        int skipCurrent = std::max(left.first, left.second) + std::max(right.first, right.second);
        
        return {robCurrent, skipCurrent};
    }
    
public:
    int rob(TreeNode* root) {
        auto result = robSub(root);
        return std::max(result.first, result.second);
    }
};
```

### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is visited once.
- **Space Complexity**: O(h), where h is the height of the binary tree due to the recursion stack. In the worst case (skewed tree), h can be n.

