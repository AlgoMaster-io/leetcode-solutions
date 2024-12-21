# [Leetcode 1110: Delete Nodes And Return Forest](https://leetcode.com/problems/delete-nodes-and-return-forest/)

## Approaches
1. [Recursive Traversal and Set Operations](#approach-1)
2. [Improved Recursive Traversal with Parent Tracking](#approach-2)

---

## Approach 1: Recursive Traversal and Set Operations

### Intuition:
The problem can be tackled using a tree traversal technique where we need to identify nodes to delete and manage the resultant forest. We’ll perform a recursive traversal of the tree, and when a node needs to be deleted, we'll adjust pointers to unlink it from the tree, while keeping track of any immediate children as new roots of subtrees in the forest.

### Algorithm:
1. Convert the list of nodes to delete into a set for O(1) lookup.
2. Define a recursive function that traverses the tree:
    - If the current node is `null`, return `null`.
    - Recursively process the left and right children.
    - If the current node needs to be deleted (is in the delete set):
        - Append its non-null children to the resulting forest as new roots.
        - Return `null` because this node is deleted and shouldn’t be attached to its parent.
    - If the node is not deleted, just return it back, linking the correctly adjusted subtrees.
3. Start the recursion from the root. If the root node isn't deleted, add it as a part of the forest.

### Complexity Analysis:
- **Time Complexity:** O(n) as we traverse each node once.
- **Space Complexity:** O(h + m), where h is the height of the tree and m is the number of nodes to delete.

```cpp
#include <vector>
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
    vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete) {
        unordered_set<int> to_delete_set(to_delete.begin(), to_delete.end());
        vector<TreeNode*> forest;
        
        // Helper recursive function
        function<TreeNode*(TreeNode*, bool)> dfs = [&](TreeNode* node, bool is_root) -> TreeNode* {
            if (!node) return nullptr;
            
            // If it's root and not to be deleted, add to forest
            bool deleted = (to_delete_set.count(node->val) > 0);
            if (is_root && !deleted) {
                forest.push_back(node);
            }
            
            // Recursively process left and right children
            node->left = dfs(node->left, deleted);
            node->right = dfs(node->right, deleted);
            
            // Return null if current node is deleted, otherwise return node
            return deleted ? nullptr : node;
        };
        
        dfs(root, true);
        
        return forest;
    }
};
```

---

## Approach 2: Improved Recursive Traversal with Parent Tracking

### Intuition:
Enhancing the straightforward recursive approach by explicitly handling node deletions and root tracking helps consolidate the decision process, reducing unnecessary traversal. This version leverages the parent's orientation to decide whether a node should be part of the forest if its parent node is deleted.

### Algorithm:
1. Use the same strategy with a set for deletion checks.
2. Implement a recursive function, maintaining both deletion context and forest roots:
    - Perform similar steps as Approach 1, but optimize by directly managing pointers in recursive descent rather than conditional checks post recursion.
3. Initially attempt to add the root unless it’s marked for deletion.

### Complexity Analysis:
- **Time Complexity:** O(n), similar to Approach 1.
- **Space Complexity:** O(h + m), similar constraints apply.

```cpp
#include <vector>
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
    vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete) {
        unordered_set<int> delete_set(to_delete.begin(), to_delete.end());
        vector<TreeNode*> result;
        root = delNodesHelper(root, delete_set, result);
        if (root != nullptr) {
            result.push_back(root);
        }
        return result;
    }
    
private:
    TreeNode* delNodesHelper(TreeNode* node, unordered_set<int>& delete_set, vector<TreeNode*>& result) {
        if (node == nullptr) return nullptr;
        
        // Recur for left and right subtrees
        node->left = delNodesHelper(node->left, delete_set, result);
        node->right = delNodesHelper(node->right, delete_set, result);
        
        // If current node is to be deleted
        if (delete_set.count(node->val)) {
            if (node->left) result.push_back(node->left);
            if (node->right) result.push_back(node->right);
            return nullptr;
        }
        
        return node;
    }
};
```
Both approaches provide a systematic technique to solve the problem, handling edge cases naturally with recursion while ensuring that the solution remains efficient with respect to both time and space.

