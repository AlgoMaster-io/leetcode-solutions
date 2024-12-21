[Leetcode Problem 199: Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

**Approach Navigation:**
1. [Breadth-First Search (BFS) Approach](#bfs-approach)
2. [Depth-First Search (DFS) Approach](#dfs-approach)

---

### BFS Approach

The problem asks for the right view of a binary tree, meaning the last node visible when looking at each level from right to left. A Breadth-First Search (BFS) is a natural approach since BFS explores nodes level by level.

#### Intuition:
- Traverse the tree level by level using a queue.
- For each level, identify the last node to store it as part of the right view.
- Using a queue helps manage traversal and keep track of level limits conveniently.

#### Solution:

```cpp
#include <iostream>
#include <vector>
#include <queue>
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
    vector<int> rightSideView(TreeNode* root) {
        vector<int> rightView;
        if (!root) return rightView;  // Empty tree

        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int levelSize = q.size();
            TreeNode* node = nullptr;
            // Traverse all nodes of the current level
            for (int i = 0; i < levelSize; ++i) {
                node = q.front();
                q.pop();
                
                // Push left and right children of current node if they exist
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            // Add the last node seen on this level to the right view
            if (node) rightView.push_back(node->val);
        }

        return rightView;
    }
};
```

**Time Complexity:** O(n), where n is the number of nodes in the tree. Each node is visited exactly once.

**Space Complexity:** O(n), for the queue which can hold at most the number of nodes in the widest level of the tree.

---

### DFS Approach

Instead of level order traversal, a depth-first search can also be used with a focus on traversing the right side of the tree first.

#### Intuition:
- Traverse the tree using DFS, preferring the right child over the left child.
- Record new levels visited and update the right view as the traversal deepens.

#### Solution:

```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> rightView;
        dfs(root, 0, rightView);
        return rightView;
    }

private:
    void dfs(TreeNode* node, int level, vector<int>& rightView) {
        if (node == nullptr) return;  // Base case: node is null

        // If visiting a new level for the first time, add the node's value
        if (level == rightView.size())
            rightView.push_back(node->val);
        
        // Recur on the right and left children (right first to capture right view)
        dfs(node->right, level + 1, rightView);
        dfs(node->left, level + 1, rightView);
    }
};
```

**Time Complexity:** O(n), where n is the number of nodes in the tree, since we still visit each node exactly once.

**Space Complexity:** O(h), where h is the height of the tree, due to the recursion stack (space complexity is O(n) in the worst case of a skewed tree).

Both BFS and DFS approaches are efficient, but the choice of one over the other might depend on the specific requirements or constraints of the problem environment (iterative with queue vs. recursive with call stack).

