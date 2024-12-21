[Leetcode Problem 662: Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/)

### Approaches:
- [Approach 1: Breadth-First Search (BFS) with Node Indexing - Basic Solution](#approach-1)
- [Approach 2: Depth-First Search (DFS) with Node Indexing - Optimized Space](#approach-2)

---

### Approach 1: Breadth-First Search (BFS) with Node Indexing - Basic Solution

**Intuition:**

In a binary tree, the width of a level is defined as the number of nodes between the leftmost and rightmost non-null nodes on that level. To calculate this, we can perform a Breadth-First Search (BFS) traversal using a queue. We'll pair each node with its position index, assuming the root node is at position index `0`. This will help us calculate the "width" by comparing positions of leftmost and rightmost nodes at each level.

**Steps:**
1. Use a queue to perform BFS, and store each node along with its index.
2. At each level of the tree:
   - Determine the width by subtracting the index of the first node from the index of the last node and adding 1.
   - Push children to the queue with updated indices: `left child = 2 * index` and `right child = 2 * index + 1`.
3. Keep track of the maximum width encountered.

```python
from collections import deque

def widthOfBinaryTree(root):
    if not root:
        return 0

    max_width = 0
    queue = deque([(root, 0)])  # (node, index)

    while queue:
        level_length = len(queue)
        _, first_index = queue[0]
        _, last_index = queue[-1]
        
        # Width of current level
        max_width = max(max_width, last_index - first_index + 1)
        
        for _ in range(level_length):
            node, index = queue.popleft()
            if node.left:
                queue.append((node.left, 2 * index))
            if node.right:
                queue.append((node.right, 2 * index + 1))

    return max_width
```

**Complexity Analysis:**
- **Time Complexity:** O(n), where n is the number of nodes in the binary tree. We visit each node once.
- **Space Complexity:** O(n), due to the space used by the queue to hold the nodes of the tree.

---

### Approach 2: Depth-First Search (DFS) with Node Indexing - Optimized Space

**Intuition:**

Instead of using a queue to track nodes at each level, we can use a recursive DFS approach. Using DFS, we can track the leftmost node at each level by storing its index in a list. This allows us to calculate the width without requiring a large data structure to hold all nodes.

**Steps:**
1. Traverse the tree using DFS, maintaining a current depth and the node's index.
2. At each depth, store the first (leftmost) index if not already stored.
3. Calculate the width of the level as the difference between the current node's index and the first index at that level.
4. Recursively calculate for left and right children with updated indices.

```python
def widthOfBinaryTree(root):
    leftmost_positions = {}
    max_width = 0
    
    def dfs(node, depth, index):
        nonlocal max_width
        if not node:
            return
        
        # Check if we need to set the leftmost index for the current level
        if depth not in leftmost_positions:
            leftmost_positions[depth] = index
        
        # Calculate the current width and update max_width
        current_width = index - leftmost_positions[depth] + 1
        max_width = max(max_width, current_width)
        
        # Continue with DFS
        dfs(node.left, depth + 1, 2 * index)
        dfs(node.right, depth + 1, 2 * index + 1)
        
    dfs(root, 0, 0)
    return max_width
```

**Complexity Analysis:**
- **Time Complexity:** O(n), where n is the number of nodes in the binary tree. We visit each node once.
- **Space Complexity:** O(h), where h is the height of the tree, due to the space used by recursion stack and leftmost_positions dictionary which stores one index per level.

