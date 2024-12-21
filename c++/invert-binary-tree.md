# [LeetCode 226: Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

## Approaches:
- [Approach 1: Recursive Depth-First Search (DFS)](#approach-1-recursive-depth-first-search-dfs)
- [Approach 2: Iterative Breadth-First Search (BFS) using a Queue](#approach-2-iterative-breadth-first-search-bfs-using-a-queue)
- [Approach 3: Iterative Depth-First Search (DFS) using a Stack](#approach-3-iterative-depth-first-search-dfs-using-a-stack)

---

## Approach 1: Recursive Depth-First Search (DFS)

This approach uses recursion to traverse the tree. The basic idea is to swap the left and right child of all nodes in the tree.

### Intuition:

1. For the current node, swap its left and right children.
2. Recursively call the invert function on the swapped left and right children.
3. This will effectively invert the entire tree as the swapping happens at every depth of the recursion.

### Implementation:

```cpp
// Definition for a binary tree node.
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
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) return nullptr;

        // Swap the left and right children
        TreeNode* temp = root->left;
        root->left = root->right;
        root->right = temp;

        // Recursive call for left and right subtree
        invertTree(root->left);
        invertTree(root->right);

        return root;
    }
};
```

### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is visited exactly once.
- **Space Complexity**: O(h), where h is the height of the tree. This is due to the recursion stack space.

---

## Approach 2: Iterative Breadth-First Search (BFS) using a Queue

BFS can also be used to invert a binary tree iteratively using a queue to manage the nodes.

### Intuition:

1. Use a queue to perform a level-order traversal of the tree.
2. For each node, swap its left and right children.
3. Add the non-null children to the queue to process their children in subsequent levels.

### Implementation:

```cpp
#include <queue>

class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) return nullptr;

        std::queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            TreeNode* current = q.front();
            q.pop();

            // Swap the left and right children
            TreeNode* temp = current->left;
            current->left = current->right;
            current->right = temp;

            // Add children to the queue to invert them later
            if (current->left != nullptr) q.push(current->left);
            if (current->right != nullptr) q.push(current->right);
        }

        return root;
    }
};
```

### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is enqueued and dequeued exactly once.
- **Space Complexity**: O(n), because in the worst case (a complete binary tree), the queue contains all nodes on one level, which is O(n/2) = O(n).

---

## Approach 3: Iterative Depth-First Search (DFS) using a Stack

Similar to BFS, DFS can also be used to invert the binary tree using a stack.

### Intuition:

1. Utilize a stack to perform a depth-first traversal.
2. For each node processed, swap its left and right children.
3. Push the swapped children onto the stack to invert their children subsequently.

### Implementation:

```cpp
#include <stack>

class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) return nullptr;

        std::stack<TreeNode*> s;
        s.push(root);

        while (!s.empty()) {
            TreeNode* current = s.top();
            s.pop();

            // Swap the left and right children
            TreeNode* temp = current->left;
            current->left = current->right;
            current->right = temp;

            // Push children onto the stack to invert them later
            if (current->left != nullptr) s.push(current->left);
            if (current->right != nullptr) s.push(current->right);
        }

        return root;
    }
};
```

### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is pushed to and popped from the stack once.
- **Space Complexity**: O(h), where h is the height of the tree. In the worst case (skewed tree), stack size can go up to O(n).

