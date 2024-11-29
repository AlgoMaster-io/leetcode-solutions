# 297. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

## Approach 1: BFS (Level Order Traversal) Using Queue

### Solution
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(n) for storing the serialized string and queue
from collections import deque

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Codec:

    # Encodes a tree to a single string
    def serialize(self, root):
        if not root:
            return "null"  # Return "null" for an empty tree

        serialized = []
        queue = deque([root])

        while queue:
            current = queue.popleft()
            if current is None:
                serialized.append("null")
            else:
                serialized.append(str(current.val))
                queue.append(current.left)
                queue.append(current.right)

        return ','.join(serialized)

    # Decodes your encoded data to tree
    def deserialize(self, data):
        if data == "null":
            return None  # Return null for an empty tree

        nodes = data.split(',')
        root = TreeNode(int(nodes[0]))
        queue = deque([root])

        i = 1
        while queue and i < len(nodes):
            current = queue.popleft()

            if nodes[i] != "null":
                current.left = TreeNode(int(nodes[i]))
                queue.append(current.left)
            i += 1

            if i < len(nodes) and nodes[i] != "null":
                current.right = TreeNode(int(nodes[i]))
                queue.append(current.right)
            i += 1

        return root
```

## Approach 2: DFS (Preorder Traversal) Using Recursion

### Solution
```python
# Time Complexity: O(n), where n is the number of nodes in the tree
# Space Complexity: O(n) for recursion stack and serialized string
from collections import deque

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Codec:

    # Encodes a tree to a single string
    def serialize(self, root):
        def serializeHelper(node, serialized):
            if not node:
                serialized.append("null")
                return

            serialized.append(str(node.val))
            serializeHelper(node.left, serialized)
            serializeHelper(node.right, serialized)

        serialized = []
        serializeHelper(root, serialized)
        return ','.join(serialized)

    # Decodes your encoded data to tree
    def deserialize(self, data):
        def deserializeHelper(queue):
            current = queue.popleft()
            if current == "null":
                return None  # Return null for "null" nodes

            node = TreeNode(int(current))
            node.left = deserializeHelper(queue)
            node.right = deserializeHelper(queue)
            return node

        nodes = data.split(',')
        queue = deque(nodes)
        return deserializeHelper(queue)
```

