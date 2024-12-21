# [Leetcode 297: Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

## Solutions
1. [Breadth-First Search (BFS) Using Queue](#solution-1-breadth-first-search-bfs-using-queue)
2. [Depth-First Search (DFS) Preorder Using Recursion](#solution-2-depth-first-search-dfs-preorder-using-recursion)

### Solution 1: Breadth-First Search (BFS) Using Queue

The basic idea here is to use a level-order traversal approach to serialize the binary tree into a string and deserialize it back to a binary tree. In BFS, we explore nodes level by level.

**Serialize:**

- We perform a level-order traversal (BFS) using a queue.
- As we explore each node, we append its value to a result list (or string) to construct the serialized data.
- For a `None` node, we append a sentinel value such as `#` to indicate a null reference in the tree.

**Deserialize:**

- For deserialization, the string is split into parts representing node values.
- We use a queue to build the tree level by level, adding left and right children for each node as we go through the list.

```python
from collections import deque

class Codec:
    def serialize(self, root):
        """Encodes a tree to a single string."""
        if not root:
            return ""
        
        serialized_data = []
        queue = deque([root])
        
        while queue:
            node = queue.popleft()
            
            if node:
                serialized_data.append(str(node.val))
                queue.append(node.left)
                queue.append(node.right)
            else:
                serialized_data.append("#")
        
        return ','.join(serialized_data)

    def deserialize(self, data):
        """Decodes your encoded data to tree."""
        if not data:
            return None
        
        values = data.split(',')
        root = TreeNode(int(values[0]))
        queue = deque([root])
        index = 1
        
        while queue:
            node = queue.popleft()
            
            if values[index] != "#":
                node.left = TreeNode(int(values[index]))
                queue.append(node.left)
            index += 1
            
            if values[index] != "#":
                node.right = TreeNode(int(values[index]))
                queue.append(node.right)
            index += 1
        
        return root
```
**Time Complexity:** O(N), where N is the number of nodes in the tree, since we process each node exactly once.

**Space Complexity:** O(N), the size of the queue used for BFS.

### Solution 2: Depth-First Search (DFS) Preorder Using Recursion

An alternative approach to serialize the binary tree is to use DFS, particularly using preorder traversal, because it captures the layout of the tree in the sequence of root-left-right.

**Serialize:**

- We can recursively perform a preorder traversal, appending values to the string.
- Again, use a sentinel value like `#` for null children to maintain structure.

**Deserialize:**

- Use the preorder sequence to reconstruct the binary tree by maintaining a global index (or iterator state).

```python
class Codec:
    def serialize(self, root):
        """Encodes a tree to a single string."""
        
        def dfs(node):
            if not node:
                return "#"
            return f"{node.val},{dfs(node.left)},{dfs(node.right)}"
        
        return dfs(root)

    def deserialize(self, data):
        """Decodes your encoded data to tree."""
        
        def dfs(values):
            val = next(values)
            if val == "#":
                return None
            
            node = TreeNode(int(val))
            node.left = dfs(values)
            node.right = dfs(values)
            return node
        
        values = iter(data.split(','))
        return dfs(values)
```

**Time Complexity:** O(N), as we visit each node exactly once during serialization and deserialization.

**Space Complexity:** O(N), due to the recursive call stack and storage of the serialized string.

Both methods offer different perspectives for the problem and have similar complexities, but choosing between them depends on differing scenarios and potential constraints like stack limits in deep trees for the recursive solution.

