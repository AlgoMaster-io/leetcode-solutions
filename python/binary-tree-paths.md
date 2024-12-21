# [Leetcode 257: Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approaches

1. [Recursive Depth-First Search (DFS)](#approach-1-recursive-depth-first-search-dfs)
2. [Iterative Depth-First Search (DFS) using Stack](#approach-2-iterative-depth-first-search-dfs-using-stack)
3. [Iterative Breadth-First Search (BFS) using Queue](#approach-3-iterative-breadth-first-search-bfs-using-queue)

---

## Approach 1: Recursive Depth-First Search (DFS)

### Intuition

The idea is to use a recursive function to perform a depth-first search of the binary tree. For each node, we append the node's value to the current path and recursively call for left and right children. If a leaf node is encountered (i.e., a node with no left or right children), we add the path to the list of paths.

### Code

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def binaryTreePaths(root):
    def construct_paths(node, path):
        if node:
            # Append the current node value to the path
            path += str(node.val)
            if not node.left and not node.right:  # Leaf node
                # Append the entire path to paths list
                paths.append(path)
            else:
                # Otherwise, continue the path and recurse for both children
                path += '->'
                construct_paths(node.left, path)
                construct_paths(node.right, path)
                
    paths = []
    construct_paths(root, '')
    return paths
```

### Complexity Analysis
- **Time Complexity:** O(N), where N is the number of nodes in the tree. We visit each node exactly once.
- **Space Complexity:** O(N), required for the recursion stack and storing paths.

---

## Approach 2: Iterative Depth-First Search (DFS) using Stack

### Intuition

This approach simulates the recursive DFS with an explicit stack. We traverse the tree with the help of a stack and maintain current paths from root to the current node. When a leaf node is reached, we append the path to the result list.

### Code

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def binaryTreePaths(root):
    if not root:
        return []

    paths = []
    stack = [(root, str(root.val))]
    
    while stack:
        node, path = stack.pop()
        # Check if the current node is a leaf node
        if not node.left and not node.right:
            paths.append(path)
        # Push right and left child to the stack if they exist
        if node.right:
            stack.append((node.right, path + '->' + str(node.right.val)))
        if node.left:
            stack.append((node.left, path + '->' + str(node.left.val)))
    
    return paths
```

### Complexity Analysis
- **Time Complexity:** O(N), where N is the number of nodes in the tree.
- **Space Complexity:** O(N), for stack and paths storage.

---

## Approach 3: Iterative Breadth-First Search (BFS) using Queue

### Intuition

Employ a queue to perform a level-order traversal (BFS) of the tree, maintaining paths from root to current nodes. For each leaf node, append its path to the results.

### Code

```python
from collections import deque

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def binaryTreePaths(root):
    if not root:
        return []

    paths = []
    queue = deque([(root, str(root.val))])
    
    while queue:
        node, path = queue.popleft()
        # Check if the current node is a leaf node
        if not node.left and not node.right:
            paths.append(path)
        # Enqueue right and left child if they exist
        if node.left:
            queue.append((node.left, path + '->' + str(node.left.val)))
        if node.right:
            queue.append((node.right, path + '->' + str(node.right.val)))
    
    return paths
```

### Complexity Analysis
- **Time Complexity:** O(N), where N is the number of nodes in the tree.
- **Space Complexity:** O(N), for queue and paths storage.

