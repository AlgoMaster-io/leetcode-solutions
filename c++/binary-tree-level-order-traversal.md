## [Leetcode 102: Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

### Approaches
1. [Recursive DFS Approach](#recursive-dfs-approach)
2. [Iterative BFS Approach](#iterative-bfs-approach)

---

### Recursive DFS Approach

Intuition:
- The idea is to use depth-first search (DFS) recursion to traverse the tree, passing along the level of depth.
- We initiate a result vector of vectors. For each call of DFS, if the current level doesn't exist in the result, we create a new sub-vector.
- Append node values to their corresponding level sub-vectors as we perform DFS.

```cpp
class Solution {
public:
    vector<vector<int>> result;
    
    void dfs(TreeNode* node, int level) {
        if (node == nullptr) return;

        // If we reach a new level we haven't seen before, add a new vector
        if (level == result.size()) {
            result.push_back(vector<int>());
        }

        // Add the current node's value to its respective level
        result[level].push_back(node->val);
        
        // Recursive DFS calls for left and right children increasing the level
        dfs(node->left, level + 1);
        dfs(node->right, level + 1);
    }

    vector<vector<int>> levelOrder(TreeNode* root) {
        dfs(root, 0);
        return result;
    }
};
```

- **Time Complexity**: O(N) where N is the number of nodes in the tree, as each node is visited once.
- **Space Complexity**: O(N) due to recursion stack space in the average scenario (balanced tree).

---

### Iterative BFS Approach

Intuition:
- Breadth-first search (BFS) is naturally suited for level order traversal as it explores nodes by level.
- Utilize a queue to facilitate BFS, processing nodes level by level.
- For each node, we append its value to the current level's sub-vector, and queue its children for the next layer of traversal.

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (root == nullptr) return {}; // Edge case
        
        vector<vector<int>> result;
        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int levelSize = q.size();
            vector<int> currentLevel;

            // Iterate over nodes at the current level
            for (int i = 0; i < levelSize; ++i) {
                TreeNode* node = q.front();
                q.pop();
                
                // Add current node's value to the current level vector
                currentLevel.push_back(node->val);

                // Queue the next level nodes
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }
            
            // Add the current level to the result
            result.push_back(currentLevel);
        }
        
        return result;
    }
};
```

- **Time Complexity**: O(N) where N is the number of nodes in the tree, since each node is added and removed from the queue once.
- **Space Complexity**: O(N) due to space needed for the queue, which in the worst case holds all the nodes of the last level.

By employing both DFS and BFS, we've tackled the level order traversal problem from different angles, providing a comprehensive set of solutions for various scenarios.

