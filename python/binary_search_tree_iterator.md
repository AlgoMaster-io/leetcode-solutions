# 173. [Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

## Approach 1: Controlled Recursion Using Stack

### Solution
python
```python
# Time Complexity:
#   - Constructor: O(h), where h is the height of the tree
#   - next(): O(1) on average across all calls
#   - hasNext(): O(1)
# Space Complexity: O(h), where h is the height of the tree for the stack

class BSTIterator:

    def __init__(self, root):
        self.stack = []
        self._pushLeft(root)

    # Push all left children of the current node to the stack
    def _pushLeft(self, node):
        while node:
            self.stack.append(node)
            node = node.left

    # @return the next smallest number
    def next(self) -> int:
        node = self.stack.pop()  # Get the topmost node (smallest available)
        if node.right:
            self._pushLeft(node.right)  # Process the right subtree
        return node.val

    # @return whether we have a next smallest number
    def hasNext(self) -> bool:
        return len(self.stack) > 0  # Check if the stack is non-empty
```

## Approach 2: Precomputed Inorder Traversal

### Solution
python
```python
# Time Complexity:
#   - Constructor: O(n), where n is the number of nodes in the tree
#   - next(): O(1)
#   - hasNext(): O(1)
# Space Complexity: O(n), where n is the number of nodes in the tree

class BSTIterator:

    def __init__(self, root):
        self.inorder = []
        self.index = 0
        self._inorderTraversal(root)

    # Perform an inorder traversal to store the elements
    def _inorderTraversal(self, node):
        if not node:
            return
        self._inorderTraversal(node.left)
        self.inorder.append(node.val)
        self._inorderTraversal(node.right)

    # @return the next smallest number
    def next(self) -> int:
        self.index += 1
        return self.inorder[self.index - 1]

    # @return whether we have a next smallest number
    def hasNext(self) -> bool:
        return self.index < len(self.inorder)
```

## Approach 3: Morris Traversal (Space Optimized, Less Practical for Iterator)

### Solution
python
```python
# Time Complexity:
#   - Constructor: O(1) (no precomputation)
#   - next(): O(1) on average across all calls
#   - hasNext(): O(1)
# Space Complexity: O(1), no additional storage

class BSTIterator:

    def __init__(self, root):
        self.current = root
        self.predecessor = None

    # @return the next smallest number
    def next(self) -> int:
        value = -1

        while self.current:
            if not self.current.left:
                value = self.current.val
                self.current = self.current.right  # Move to the right subtree
                break
            else:
                self.predecessor = self.current.left

                # Find the rightmost node in the left subtree
                while self.predecessor.right and self.predecessor.right != self.current:
                    self.predecessor = self.predecessor.right

                if not self.predecessor.right:
                    self.predecessor.right = self.current  # Create a temporary link
                    self.current = self.current.left
                else:
                    self.predecessor.right = None  # Remove the temporary link
                    value = self.current.val
                    self.current = self.current.right  # Move to the right subtree
                    break

        return value

    # @return whether we have a next smallest number
    def hasNext(self) -> bool:
        return self.current is not None
```

