# [Leetcode 199: Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

## Approaches:

1. [Depth-First Search (DFS) Preorder Traversal](#depth-first-search-dfs-preorder-traversal)
2. [Breadth-First Search (BFS)](#breadth-first-search-bfs)

---

### Depth-First Search (DFS) Preorder Traversal

Intuition: The idea here is to traverse the tree using a modified preorder traversal approach (right node first, then left node). During this traversal, for each level we visit, we add the node that we visit first (which will be the rightmost node for that level due to our traversal order) to the result list. This way, we ensure that for each level, the rightmost element is processed first.

```python
def rightSideView(root: Optional[TreeNode]) -> List[int]:
    def dfs(node, level):
        if not node:
            return
        # If this is the first time we're visiting this level, add the node value
        if level == len(view):
            view.append(node.val)
        # First visit the right, then the left node
        dfs(node.right, level + 1)
        dfs(node.left, level + 1)

    view = []
    dfs(root, 0)
    return view
```

**Detailed Comments:**
- Define a helper function `dfs(node, level)` to perform the depth-first search.
- Start by checking if the current node is `None`. If so, simply return since there's nothing to process.
- Check if this level is encountered for the first time by comparing it against the length of the `view`. If so, append the node's value.
- Recursively call `dfs` for the right child first, then the left child.

**Time Complexity:** O(N), where N is the number of nodes in the tree. Each node is visited exactly once.

**Space Complexity:** O(H), where H is the height of the tree. This space is used on the call stack due to recursion.

---

### Breadth-First Search (BFS)

Intuition: This approach involves using a queue to facilitate level-order traversal (breadth-first). For each level, the last node encountered is the rightmost node for that level, which is exactly what we need to include in our result list.

```python
from collections import deque

def rightSideView(root: Optional[TreeNode]) -> List[int]:
    if not root:
        return []
    
    view = []
    queue = deque([root])

    while queue:
        level_length = len(queue)
        for i in range(level_length):
            node = queue.popleft()
            # If it’s the last node in this level, add its value to the view
            if i == level_length - 1:
                view.append(node.val)
            # Add child nodes of the current node in the queue for the next level
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
                
    return view
```

**Detailed Comments:**
- Initialize a `queue` with the root node if the root exists.
- While the queue is not empty, process nodes level by level.
- Record the `level_length` to iterate through all nodes at the current level.
- For each node, check if it’s the last node of the current level (i.e., `i == level_length - 1`), and if so, append the node's value to the `view`.
- Add the left and right children to the queue for processing the next level.

**Time Complexity:** O(N), where N is the number of nodes in the tree. Each node is processed at most once.

**Space Complexity:** O(W), where W is the maximum width of the tree. This is required for the queue used in level-order traversal.

