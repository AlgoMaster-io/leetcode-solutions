# 133. [Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approach: Depth-First Search (DFS) with HashMap

### Solution
```python
# Time Complexity: O(n), where n is the number of nodes in the graph
# Space Complexity: O(n), for the HashMap and recursion stack
from collections import defaultdict

class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

class Solution:
    def __init__(self):
        self.map = {}

    def cloneGraph(self, node):
        if node is None:
            return None

        # If the node is already cloned, return the cloned node
        if node in self.map:
            return self.map[node]

        # Clone the current node
        clone = Node(node.val)
        self.map[node] = clone

        # Recursively clone all neighbors
        for neighbor in node.neighbors:
            clone.neighbors.append(self.cloneGraph(neighbor))

        return clone
```

