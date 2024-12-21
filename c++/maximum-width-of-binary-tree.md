[Leetcode Problem 662: Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/)

## Approaches
1. [Level Order Traversal using BFS](#approach-1-level-order-traversal-using-bfs)
2. [DFS with Tracking Indices](#approach-2-dfs-with-tracking-indices)

---

### Approach 1: Level Order Traversal using BFS 

To find the maximum width of a binary tree, we can perform a level order traversal using BFS. During the traversal, we keep track of the position:index of each node within its level. The width of a level will be defined by the difference between the rightmost and leftmost indices plus one.

#### Intuition:
- We can imagine each node having a position value if arranged in a complete binary tree. The root has position 1.
- For any node at position `pos`, its left child will be at `2 * pos` and right child at `2 * pos + 1`.
- For each level during BFS, record the position of the first and last node. The difference gives the width of that level.

#### Steps:
1. Maintain a queue which stores pairs of TreeNode and their position index.
2. Perform a standard BFS traversal.
3. At each level, store the start index of the first node and end index of the last node in that level.
4. Update the maximum width using the end index minus start index plus 1 for each level.
5. Return the maximum width encountered.

```cpp
#include <iostream>
#include <queue>
using namespace std;

// Define TreeNode structure
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

int widthOfBinaryTree(TreeNode* root) {
    if (!root) return 0;

    queue<pair<TreeNode*, unsigned long long>> q;
    q.push({root, 1});
    unsigned long long maxWidth = 0;

    while (!q.empty()) {
        int size = q.size();
        unsigned long long start = q.front().second; // Starting index of the level
        unsigned long long end = q.back().second;   // Ending index of the level
        maxWidth = max(maxWidth, end - start + 1);

        for (int i = 0; i < size; i++) {
            auto nodePair = q.front();
            q.pop();
            TreeNode* node = nodePair.first;
            unsigned long long index = nodePair.second;
            // Calculate indices for children
            if (node->left) {
                q.push({node->left, 2 * index});
            }
            if (node->right) {
                q.push({node->right, 2 * index + 1});
            }
        }
    }

    return maxWidth;
}
```
**Time Complexity**: O(N), where N is the number of nodes in the tree. We visit each node once.

**Space Complexity**: O(N), maximum space used by the queue is the maximum number of nodes at any level in the tree.

---

### Approach 2: DFS with Tracking Indices

We can also use a depth-first search approach to solve this problem, where we recursively compute the potential position of each node and keep track of the first and last position seen at each level.

#### Intuition:
- Use a DFS traversal while assigning indices to nodes as if placed in a complete binary tree.
- Use a hashmap to store the first index seen at each depth to calculate the potential width.
- Update the maximum width when visiting a new node at the same level.

#### Steps:
1. Perform a DFS traversal.
2. Maintain a map to keep track of the first column position seen at each depth.
3. For each node, update the width to be the largest difference between the current index and the first index seen at that depth plus one.
4. Recursively process the left and right children.

```cpp
#include <iostream>
#include <map>
using namespace std;

void dfs(TreeNode* node, unsigned long long index, int depth, unsigned long long &maxWidth, map<int, unsigned long long> &firstColIndexMap) {
    if (!node) return;
    
    // If encountering the depth first time, record the index
    if (firstColIndexMap.find(depth) == firstColIndexMap.end()) {
        firstColIndexMap[depth] = index;
    }
    
    // Calculate the width for this depth
    maxWidth = max(maxWidth, index - firstColIndexMap[depth] + 1);
    
    // Recur for left and right child
    dfs(node->left, 2 * index, depth + 1, maxWidth, firstColIndexMap);
    dfs(node->right, 2 * index + 1, depth + 1, maxWidth, firstColIndexMap);
}

int widthOfBinaryTree(TreeNode* root) {
    unsigned long long maxWidth = 0;
    map<int, unsigned long long> firstColIndexMap; // First index of each level
    dfs(root, 1, 0, maxWidth, firstColIndexMap);
    return maxWidth;
}
```
**Time Complexity**: O(N), where N is the number of nodes in the tree as each node is visited once.

**Space Complexity**: O(H), where H is the height of the tree due to recursion stack space and extra map storage for depth indices.

---

