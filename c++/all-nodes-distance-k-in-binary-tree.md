# [Leetcode 863: All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)

In the problem, we're given the root of a binary tree, a target node, and an integer `K`. We must find all nodes that are a distance `K` from the target node.

## Approaches

- [Approach 1: BFS and Backtracking](#approach-1-bfs-and-backtracking)
- [Approach 2: Two-Pass Approach](#approach-2-two-pass-approach)

### Approach 1: BFS and Backtracking

**Intuition:**

The problem can be treated as a graph problem where each node is a graph node, and each tree edge is a graph edge. Our task is to find all nodes that are `K` edges away from the target node in this graph.

To achieve this:
1. Convert the tree into an undirected graph by treating parent-child relationships as bidirectional edges.
2. Use Breadth-First Search (BFS) to perform a level order traversal starting from the target node to find all nodes that are `K` edges away.

**Steps:**

- First, we need to record parent pointers for each node - this allows us to traverse the tree upwards.
- Convert the tree into a graph-like structure by establishing links in both the child-parent and parent-child directions.
- Utilize BFS starting from the target node to find all nodes at a distance `K`.

```cpp
#include <vector>
#include <unordered_map>
#include <queue>
#include <unordered_set>

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
    vector<int> distanceK(TreeNode* root, TreeNode* target, int K) {
        // Step 1: Create a map of node to its parent
        unordered_map<TreeNode*, TreeNode*> parentMap;
        createParentMap(root, parentMap, nullptr);

        // Step 2: Use a queue to perform BFS and find nodes at distance K
        queue<TreeNode*> q;
        q.push(target);

        // Keep track of visited nodes to prevent cycles
        unordered_set<TreeNode*> visited;
        visited.insert(target);

        int currentDistance = 0;

        while (!q.empty()) {
            int size = q.size();
            // If current distance is K, collect all nodes at this level
            if (currentDistance == K) {
                vector<int> result;
                for (int i = 0; i < size; ++i) {
                    result.push_back(q.front()->val);
                    q.pop();
                }
                return result;
            }

            // Process nodes at the current level
            for (int i = 0; i < size; ++i) {
                TreeNode* node = q.front();
                q.pop();

                // Check neighboring nodes (left child, right child, parent)
                if (node->left && !visited.count(node->left)) {
                    q.push(node->left);
                    visited.insert(node->left);
                }
                if (node->right && !visited.count(node->right)) {
                    q.push(node->right);
                    visited.insert(node->right);
                }
                if (parentMap[node] && !visited.count(parentMap[node])) {
                    q.push(parentMap[node]);
                    visited.insert(parentMap[node]);
                }
            }
            currentDistance++;
        }

        // In case no nodes found at distance K
        return {};
    }

private:
    void createParentMap(TreeNode* node, unordered_map<TreeNode*, TreeNode*>& parentMap, TreeNode* parent) {
        if (!node) return;

        parentMap[node] = parent;

        createParentMap(node->left, parentMap, node);
        createParentMap(node->right, parentMap, node);
    }
};
```

**Time Complexity:** O(N) where N is the number of nodes in the tree, because we visit each node once.
**Space Complexity:** O(N) which accounts for the queue and the parent map.

### Approach 2: Two-Pass Approach

**Intuition:**

A more intuitive approach can utilize two passes:
1. First, find all ancestors of the target node, keeping track of the distance to each ancestor.
2. In a second pass, use DFS to find nodes that are `K` distance away using the distance info from ancestors and direct sub-tree exploration.

**Steps:**

- First pass: Traverse the tree to gather the distance info from the target to all of its ancestors.
- Second pass: Use this distance info to explore nodes at distance `K`.

```cpp
// This approach assumes a helper function `findPath` is defined to trace target to root path
// due to limitations of output, detailed comments are omitted, focusing on structure and complexity.

#include <vector>
#include <unordered_set>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    vector<int> distanceK(TreeNode* root, TreeNode* target, int K) {
        vector<TreeNode*> path;
        findPath(root, target, path);

        vector<int> result;
        for (int i = 0; i < path.size(); ++i) {
            findNodesAtDistance(path[i], K - i, result, i == 0 ? nullptr : path[i - 1]);
        }
        return result;
    }

private:
    bool findPath(TreeNode* root, TreeNode* target, vector<TreeNode*>& path) {
        if (!root) return false;
        
        path.push_back(root);
        if (root == target) return true;
        
        if (findPath(root->left, target, path) || findPath(root->right, target, path)) return true;
        
        path.pop_back();
        return false;
    }
    
    void findNodesAtDistance(TreeNode* node, int K, vector<int>& result, TreeNode* blocker) {
        if (!node || K < 0 || node == blocker) return;
        
        if (K == 0) {
            result.push_back(node->val);
            return;
        }
        
        findNodesAtDistance(node->left, K - 1, result, blocker);
        findNodesAtDistance(node->right, K - 1, result, blocker);
    }
};
```

**Time Complexity:** O(N) because each node is visited only once in both passes.
**Space Complexity:** O(N) due to the recursive stack during DFS and the path storage.

This method keeps the scope restricted to relevant subtree exploration and uses recursive calls effectively to achieve desired results.

