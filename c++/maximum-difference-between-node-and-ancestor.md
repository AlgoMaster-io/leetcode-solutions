### [Leetcode 1026: Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/)

#### Approaches:
1. [Recursive Depth-First Search (DFS) with Ancestor Tracking](#solution-1)

---

### Solution 1: Recursive Depth-First Search (DFS) with Ancestor Tracking

**Intuition:**

The goal is to determine the greatest absolute difference between the value of any node and the value of any ancestor of that node in a binary tree. This can be effectively achieved using a Depth-First Search (DFS) approach. 

During the DFS traversal, we keep track of the maximum and minimum values seen along the path from the root to the current node. For each node, the maximum difference with any ancestor would be the greater of the differences between the node and the maximum ancestor, or the node and the minimum ancestor. By recursively calculating this value for left and right children, we can propagate up the maximum observed difference.

**Steps:**
1. Recursively traverse the tree starting from the root.
2. For each node, keep track of the current path's maximum and minimum values.
3. At each node, calculate potential differences with the current path’s maximum and minimum values.
4. Recursively calculate the differences for the left and right subtrees and update the path’s maximum and minimum accordingly.
5. The result is the maximum of these calculated differences across all paths in the tree.

**Pseudocode:**

```cpp
class Solution {
public:
    int maxAncestorDiff(TreeNode* root) {
        // Start DFS traversal from the root with the initial max and min both set to root's value.
        return dfs(root, root->val, root->val);
    }

private:
    int dfs(TreeNode* node, int currentMax, int currentMin) {
        // Base condition: If the node is null, the difference would be zero.
        if (!node) return 0;

        // Calculate the maximum difference at the current node.
        int potentialDiff = max(abs(currentMax - node->val), abs(currentMin - node->val));

        // Update current max and min based on the current node's value.
        currentMax = max(currentMax, node->val);
        currentMin = min(currentMin, node->val);

        // Recursively calculate the differences for the left and right subtrees.
        int leftDiff = dfs(node->left, currentMax, currentMin);
        int rightDiff = dfs(node->right, currentMax, currentMin);

        // The maximum difference found in either subtree or at the current node.
        return max(potentialDiff, max(leftDiff, rightDiff));
    }
};
```

**Time Complexity:** O(N), where N is the number of nodes in the binary tree, since each node is visited once.

**Space Complexity:** O(H), where H is the height of the binary tree, due to the recursive call stack. In the worst case (skewed tree), this could be O(N).

This approach efficiently considers all paths in the tree but does so with minimal extra space, aside from the stack space used during the recursive calls. Each node is evaluated with respect to its ancestors in constant time by simply comparing values along the path.

