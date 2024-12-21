# [Leetcode 236: Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

## Approaches:
- [Approach 1: Recursive Approach with DFS](#approach-1-recursive-approach-with-dfs)
- [Approach 2: Iterative Approach Using Parent Pointers](#approach-2-iterative-approach-using-parent-pointers)

## Approach 1: Recursive Approach with DFS

### Intuition:
The problem of finding the lowest common ancestor (LCA) can be approached by exploring the binary tree using Depth-First Search (DFS). The idea is to explore subtrees and determine if the nodes `p` and `q` are located in different branches or within the same subtree, which would define the LCA.

### Steps:
1. **Base Case:** If the current node is null or matches one of the two nodes (`p` or `q`), return the current node.
2. **Recursive Search:** Perform a DFS on both left and right children.
3. **Determination of LCA:**
    - If one node is found in the left subtree and the other is found in the right subtree, the current node is the LCA.
    - If both nodes are found in one side (left or right), propagate the node upwards.

### C++ Code:

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // Base case: if the current node is NULL or is one of the target nodes
        if (!root || root == p || root == q) return root;

        // Search in the left and right subtree
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        // If both left and right are not null, then this node is the LCA
        if (left && right) return root;

        // If only one side is not null, return the non-null side
        return left ? left : right;
    }
};
```

### Time Complexity:
- **O(N)**: We visit each node once in the binary tree.

### Space Complexity:
- **O(H)**: The height of the tree due to recursion stack; in the worst case, H = N for a skewed tree.

## Approach 2: Iterative Approach Using Parent Pointers

### Intuition:
If storing parent pointers allows us to easily trace paths back to the root from both nodes `p` and `q`, the problem of finding LCA simplifies to finding an intersection point in these paths.

### Steps:
1. **Build the Parent Map:** Use a stack to perform DFS, maintain a map from children to parents.
2. **Trace Path for One Node:** Trace back from node `p` to the root, adding each ancestor to a set.
3. **Find First Common Ancestor:** Trace from node `q` back to the root, checking if any node is already in the set.

### C++ Code:

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // Map each node to its parent
        unordered_map<TreeNode*, TreeNode*> parentMap;
        stack<TreeNode*> dfsStack;
        parentMap[root] = nullptr;
        dfsStack.push(root);

        // Fill parentMap during DFS traversal
        while (!parentMap.count(p) || !parentMap.count(q)) {
            TreeNode* current = dfsStack.top();
            dfsStack.pop();

            if (current->left) {
                parentMap[current->left] = current;
                dfsStack.push(current->left);
            }
            if (current->right) {
                parentMap[current->right] = current;
                dfsStack.push(current->right);
            }
        }

        // Collect ancestors of p
        unordered_set<TreeNode*> ancestors;
        while (p) {
            ancestors.insert(p);
            p = parentMap[p];
        }

        // Find the common ancestor in q's path
        while (!ancestors.count(q)) {
            q = parentMap[q];
        }

        return q;
    }
};
```

### Time Complexity:
- **O(N)**: Where N is the number of nodes in the binary tree since every node and parent is visited once.

### Space Complexity:
- **O(N)**: To store the parent pointers and ancestors set.

