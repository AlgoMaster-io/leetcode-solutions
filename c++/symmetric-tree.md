# [Leetcode 101: Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

## Approaches:
- [Approach 1: Recursive Solution (DFS)](#approach-1-recursive-solution-dfs)
- [Approach 2: Iterative Solution (BFS using Queue)](#approach-2-iterative-solution-bfs-using-queue)

### Approach 1: Recursive Solution (DFS)

**Intuition:**

A tree is symmetric if the left subtree is a mirror reflection of the right subtree. Therefore, a binary tree is symmetric if the left child of the left subtree is the same as the right child of the right subtree, and the right child of the left subtree is the same as the left child of the right subtree.

The recursive solution is derived from this very definition: to check if two trees are mirror images, their root values must be the same, and their subtrees must be mirror images of each other.

**Steps:**
1. Define a helper function `isMirror`, which checks if two trees are mirror images.
2. Check if the root is null. If yes, it is symmetric by definition.
3. Use recursion in `isMirror` to compare:
   - The left of one tree with the right of the other (and vice versa).

```cpp
class Solution {
public:    
    bool isMirror(TreeNode* t1, TreeNode* t2) {
        // If both subtrees are empty, then they are mirror images
        if (t1 == nullptr && t2 == nullptr) return true;
        // If only one of them is empty, they are not mirrors
        if (t1 == nullptr || t2 == nullptr) return false;
        // The two roots have the same value and their respective subtrees are mirrors
        return (t1->val == t2->val)
            && isMirror(t1->right, t2->left)
            && isMirror(t1->left, t2->right);
    }

    bool isSymmetric(TreeNode* root) {
        // A tree is a mirror of itself
        return isMirror(root, root);
    }
};
```

**Time Complexity:** O(n), where n is the number of nodes in the tree because each node is visited once.

**Space Complexity:** O(h), where h is the height of the tree. In the worst case of a completely unbalanced tree, the space complexity can be O(n).

### Approach 2: Iterative Solution (BFS using Queue)

**Intuition:**

Instead of using recursion, we can use an iterative approach with a queue (or two stacks) to simulate the mirror checking. In this approach, we traverse the two trees simultaneously using a queue, ensuring nodes are appropriately paired.

**Steps:**
1. Use a queue to handle node pairs.
2. Start by enqueueing the root node twice (to simulate comparing the tree with itself).
3. While the queue is not empty, dequeue two nodes and check:
   - If both nodes are null, continue (they're symmetric).
   - If one is null and the other isn't, or values don't match, they're not symmetric.
   - Enqueue the children in opposite order:
     - left child of one with right child of the other, and vice versa.

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (!root) return true; // A null tree is symmetric

        queue<TreeNode*> q;
        q.push(root);
        q.push(root);

        while (!q.empty()) {
            // Dequeue two nodes
            TreeNode* t1 = q.front(); q.pop();
            TreeNode* t2 = q.front(); q.pop();

            // If both are null, they are symmetrical
            if (!t1 && !t2) continue;
            // If one is null or the values don't match, they're not symmetric
            if (!t1 || !t2 || t1->val != t2->val) return false;

            // Enqueue children in mirrored order
            q.push(t1->left);
            q.push(t2->right);
            q.push(t1->right);
            q.push(t2->left);
        }
        return true; // All checks passed
    }
};
```

**Time Complexity:** O(n), since we visit every node.

**Space Complexity:** O(n), needed for the queue which, in the worst case, can contain all nodes at the deepest level.

