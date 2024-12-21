# [LeetCode 863: All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)

## Table of Contents
1. [Approach 1: BFS for Parents and DFS for Result](#approach-1)
2. [Approach 2: BFS from Target Node](#approach-2)


## Approach 1: BFS for Parents and DFS for Result

### Intuition
- The problem involves finding all nodes that are k distance away from a given target in a binary tree.
- The most straightforward solution involves first establishing a way to navigate from any node to its parent since binary trees do not store parent references by default.
- Once we have established parent references for each node, we can use a Depth-First Search (DFS) strategy starting from the target node to find the nodes exactly k distance away.

### Steps
1. **Establish Parent Pointers**: Do a Breadth-First Search (BFS) to map each node to its parent. This allows backward traversal which is necessary when finding nodes k distance away.
2. **DFS from Target**: Use Depth-First Search (DFS) from the target node with the ability to move to children and parent, while keeping track of visited nodes to prevent cycles.

### Code

```python
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def distanceK(root: Optional[TreeNode], target: TreeNode, k: int) -> List[int]:
    # Dictionary to store parent references
    parent_map = {}
    
    # Helper function to populate parent_map
    def populate_parents(node, parent=None):
        if node:
            parent_map[node] = parent
            populate_parents(node.left, node)
            populate_parents(node.right, node)

    # Perform BFS to populate parent_map
    populate_parents(root)
    
    # List to store result nodes at distance K
    result = []
    # Set to store visited nodes to prevent cycles
    visited = set()

    # DFS function to find nodes at distance K
    def dfs(node, distance):
        if node is None or node in visited:
            return
        
        visited.add(node)
        
        # If distance is zero, append the current node value to result
        if distance == k:
            result.append(node.val)
            return
        
        # Move to the left child, right child, or the parent node
        if node.left: dfs(node.left, distance + 1)
        if node.right: dfs(node.right, distance + 1)
        if node in parent_map and parent_map[node]: dfs(parent_map[node], distance + 1)

    # Start DFS from the target node
    dfs(target, 0)
    
    return result
```

### Complexity
- **Time Complexity**: `O(N)` where `N` is the number of nodes in the tree since we potentially visit all nodes to set up parent pointers and find nodes at distance k.
- **Space Complexity**: `O(N)` due to the storage required for parent map and visited nodes.

## Approach 2: BFS from Target Node

### Intuition
- In this approach, we utilize a Breadth-First Search (BFS) to find all nodes at distance k from the target node, leveraging the tree structure and additional parent pointers for backward traversal.
- BFS naturally traverses level by level, making it efficient for this problem where distance can be considered as levels.

### Steps
1. **Establish Parent Pointers**: The same initial step to map each node to its parent.
2. **BFS from Target**: Initiate a BFS from the target node considering all neighbors (left, right children, and parent) until the desired distance is reached.

### Code

```python
from typing import List, Optional
from collections import deque

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def distanceK(root: Optional[TreeNode], target: TreeNode, k: int) -> List[int]:
    # Dictionary to store parent references
    parent_map = {}
    
    # Helper function to populate parent_map
    def populate_parents(node, parent=None):
        if node:
            parent_map[node] = parent
            populate_parents(node.left, node)
            populate_parents(node.right, node)

    # Perform BFS to populate parent_map
    populate_parents(root)
    
    # Initialize a queue for BFS
    queue = deque([(target, 0)])
    # Set to store visited nodes to prevent cycles
    visited = {target}
    result = []
    
    # Execute BFS
    while queue:
        node, distance = queue.popleft()
        
        if distance == k:
            result.append(node.val)
            continue
        
        # Explore neighbors: left child, right child, and parent
        for neighbor in (node.left, node.right, parent_map[node]):
            if neighbor and neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, distance + 1))
    
    return result
```

### Complexity
- **Time Complexity**: `O(N)`
- **Space Complexity**: `O(N)`, primarily due to the queue and parent map.

Choosing between BFS and DFS depends on preference and familiarity. BFS gives a level-by-level exploration, which naturally fits problems involving distances, whereas DFS provides straightforward recursive logic. In this problem, both yield similar complexity, thus either can be applied effectively.

