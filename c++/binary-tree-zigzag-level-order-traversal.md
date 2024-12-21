# [Leetcode 103: Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Approaches
1. [Breadth-First Search (BFS) with Level Tracking](#approach-1-bfs-with-level-tracking)
2. [Optimized BFS with Deque](#approach-2-optimized-bfs-with-deque)

---

## Approach 1: BFS with Level Tracking

### Intuition
This approach uses a breadth-first search (BFS) to traverse the tree level by level while maintaining the order of the levels in a zigzag manner. We maintain a queue to facilitate level-wise traversal. For even-indexed levels, we insert nodes normally, and for odd-indexed levels, we append nodes in reverse order.

### Steps:
1. Perform a standard BFS using a queue.
2. Keep track of the current level.
3. For each level, determine if it is an odd or even level.
4. If odd level, reverse the node values before adding them to the result.
5. Append the level's node values to the result list.

### Time Complexity
- **O(N)**, where N is the number of nodes in the tree since each node is visited once.

### Space Complexity
- **O(W)**, where W is the maximum width of the tree which can be in the order of N/2 for a complete binary tree.

```cpp
#include <vector>
#include <queue>
#include <algorithm>

struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(): val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right): val(x), left(left), right(right) {}
};

std::vector<std::vector<int>> zigzagLevelOrder(TreeNode* root) {
    std::vector<std::vector<int>> result;
    if (!root) return result;

    std::queue<TreeNode*> nodeQueue;
    nodeQueue.push(root);
    bool leftToRight = true;

    while (!nodeQueue.empty()) {
        int size = nodeQueue.size();
        std::vector<int> level(size);

        for (int i = 0; i < size; ++i) {
            TreeNode* node = nodeQueue.front();
            nodeQueue.pop();

            // Find position to fill node's value depending on traversal direction
            int index = leftToRight ? i : (size - 1 - i);

            // Insert the node's value at the calculated index
            level[index] = node->val;

            if (node->left) nodeQueue.push(node->left);
            if (node->right) nodeQueue.push(node->right);
        }

        // After processing the current level, toggle the direction.
        leftToRight = !leftToRight;
        result.push_back(level);
    }

    return result;
}
```

---

## Approach 2: Optimized BFS with Deque

### Intuition
Instead of reversing the order for every alternate level, we can use a deque to add elements in a zigzag order efficiently. For the current level, if adding left-to-right, we push to the back of the deque, and if adding right-to-left, we push to the front.

### Steps:
1. Use a BFS approach with a deque.
2. Utilize the deque to decide the position of the elements on the fly.
3. Alternate between insertion to the front and back based on the level.

### Time Complexity
- **O(N)**, since each node is processed once.

### Space Complexity
- **O(W)**, where W is the maximum number of nodes at any level in the tree.

```cpp
#include <vector>
#include <deque>

std::vector<std::vector<int>> zigzagLevelOrder(TreeNode* root) {
    std::vector<std::vector<int>> result;
    if (!root) return result;

    std::deque<TreeNode*> deq;
    deq.push_back(root);
    bool leftToRight = true;

    while (!deq.empty()) {
        int size = deq.size();
        std::vector<int> level(size);

        for (int i = 0; i < size; ++i) {
            if (leftToRight) {
                TreeNode* node = deq.front();
                deq.pop_front();
                level[i] = node->val;
                if (node->left) deq.push_back(node->left);
                if (node->right) deq.push_back(node->right);
            } else {
                TreeNode* node = deq.back();
                deq.pop_back();
                level[i] = node->val;
                if (node->right) deq.push_front(node->right);
                if (node->left) deq.push_front(node->left);
            }
        }

        leftToRight = !leftToRight;
        result.push_back(level);
    }

    return result;
}
```

In this solution, both deque operations and list insertions are optimal for zigzag ordering without the need to reverse the order after each level.

