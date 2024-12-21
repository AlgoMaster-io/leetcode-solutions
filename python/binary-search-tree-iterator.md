# [Leetcode 173: Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approaches
1. [Inorder Traversal Using Extra Space](#approach-1-inorder-traversal-using-extra-space)
2. [Controlled Morris Traversal](#approach-2-controlled-morris-traversal)
3. [Stack and Controlled Iterative Inorder Traversal - Optimized](#approach-3-stack-and-controlled-iterative-inorder-traversal---optimized)

---

## Approach 1: Inorder Traversal Using Extra Space

### Intuition
The simplest way to implement a binary search tree iterator is to perform an inorder traversal of the tree and store the results in a list. When `next()` is called, we simply return the next element in the list. This approach requires more space to store the elements of the tree in a list.

### Steps
1. Perform an inorder traversal of the BST and store the values in a list.
2. Use an index to iterate through this list for the `next()` calls.
3. Use the list’s length for `hasNext()` to check if there are any more elements left.

### Code
```python
class BSTIterator:
    def __init__(self, root):
        # Initialize the list and perform inorder traversal
        self.nodes_list = []
        self.index = -1
        self._inorder_traversal(root)
    
    def _inorder_traversal(self, node):
        # Helper function to perform inorder traversal and fill the list
        if node is None:
            return
        self._inorder_traversal(node.left)
        self.nodes_list.append(node.val)
        self._inorder_traversal(node.right)

    def next(self):
        # Move the index and return the next element
        self.index += 1
        return self.nodes_list[self.index]

    def hasNext(self):
        # Check if there are more elements in the list
        return self.index + 1 < len(self.nodes_list)
```

### Complexity
- **Time Complexity:** `O(1)` for `next()` and `hasNext()`; the traversal takes `O(n)`, where `n` is the number of nodes.
- **Space Complexity:** `O(n)` to store the nodes in a list.

---

## Approach 2: Controlled Morris Traversal

### Intuition
This approach doesn’t use extra space to store nodes, it uses the tree structure to mimic the inorder traversal. We modify the tree temporarily and restore it afterwards. Morris Traversal uses threaded binary trees to traverse the tree with space complexity of `O(1)`.

### Steps
1. Use a threaded binary tree to traverse in inorder fashion.
2. Keep track of the current position using the tree links and node values.

### Code
```python
class BSTIterator:
    def __init__(self, root):
        self.root = root
        self.current = None
        self.next_node_value = None
        self._morris_setup()

    def _morris_setup(self):
        # Advance to the first node in the inorder traversal
        current = self.root
        while current:
            if current.left is None:
                self.current = current
                return
            else:
                pred = current.left
                while pred.right is not None and pred.right != current:
                    pred = pred.right
                if pred.right is None:
                    pred.right = current
                    current = current.left
                else:
                    pred.right = None
                    self.current = current
                    return

    def next(self):
        if self.current is None:
            return None
        next_value = self.current.val
        self._morris_setup()
        return next_value

    def hasNext(self):
        return self.current is not None
```

### Complexity
- **Time Complexity:** `O(1)` for `next()` on average; `O(n)` for initial setup which runs every `next()`.
- **Space Complexity:** `O(1)` because no extra space is used apart from a couple of pointers.

---

## Approach 3: Stack and Controlled Iterative Inorder Traversal - Optimized

### Intuition
A better approach than both extensive space usage (array storage) and the complex threaded tree traversal is to use a controlled iteration with a stack. This stack holds the ancestors of the current node, which allows for a controlled traversal without altering the tree's structure or using additional space like arrays.

### Steps
1. Use a stack to keep track of the nodes to be visited.
2. Traverse left until None, pushing all nodes onto the stack.
3. `next()` pops elements from the stack and processes right sub-trees.
4. `hasNext()` checks if the stack has elements.

### Code
```python
class BSTIterator:
    def __init__(self, root):
        # Stack for the controlled traversal
        self.stack = []
        # Initialize by going to the leftmost node
        self._leftmost_inorder(root)

    def _leftmost_inorder(self, node):
        # Insert all nodes to reach the leftmost node of a subtree
        while node:
            self.stack.append(node)
            node = node.left

    def next(self):
        # Node at the top of the stack is the next element
        topmost_node = self.stack.pop()
        # If there is a right node, perform the leftmost traversal again
        if topmost_node.right:
            self._leftmost_inorder(topmost_node.right)
        return topmost_node.val

    def hasNext(self):
        # Check if the stack has any more elements
        return len(self.stack) > 0
```

### Complexity
- **Time Complexity:** `O(1)` on average for `next()` as each node is pushed and popped once.
- **Space Complexity:** `O(h)` where `h` is the height of the tree (space for stack).

This approach balances between not using additional memory and maintaining reasonable time efficiency across operations.

