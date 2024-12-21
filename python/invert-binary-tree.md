# [Leetcode 226: Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

## Approach

1. [Recursive Solution](#recursive-solution)
2. [Iterative Solution using BFS with Queue](#iterative-solution-using-bfs-with-queue)
3. [Iterative Solution using DFS with Stack](#iterative-solution-using-dfs-with-stack)

---

### Recursive Solution

The most intuitive and straightforward way to solve this problem is to use recursion. Since a binary tree is a recursive data structure, we can naturally apply a recursive approach to invert it.

**Intuition:**

1. For each node, swap its left and right children.
2. Recursively invert the left subtree.
3. Recursively invert the right subtree.

**Algorithm:**

- Base case: If the current node is `None`, return `None`.
- Swap the left and right children of the current node.
- Recursively call the function for the left and right children.

**Implementation:**

```python
class TreeNode:
    def __init__(self, value=0, left=None, right=None):
        self.val = value
        self.left = left
        self.right = right

def invertTree(root: TreeNode) -> TreeNode:
    if root is None:
        # Base case: If the node is None, there's nothing to invert
        return None
    
    # Recursively invert the right and left subtrees
    root.left, root.right = invertTree(root.right), invertTree(root.left)
    
    # After inversion return the current node
    return root
```

**Time Complexity:** O(n) where n is the number of nodes in the tree, because we visit each node exactly once.

**Space Complexity:** O(h) where h is the height of the tree due to the space used by the call stack. In the worst case, h = n, if the tree is completely skewed.

---

### Iterative Solution using BFS with Queue

Instead of using recursion, we can use an iterative approach with the help of a queue to perform a level-order traversal (BFS).

**Intuition:**

1. Use a queue to process nodes in a level order.
2. For each node, swap its left and right children.
3. Enqueue the non-null left and right children to the queue for further processing.

**Algorithm:**

- Initialize a queue and enqueue the root node.
- While the queue is not empty:
  - Dequeue a node.
  - Swap its left and right children.
  - Enqueue the non-null children.

**Implementation:**

```python
from collections import deque

def invertTreeIterativeBFS(root: TreeNode) -> TreeNode:
    if root is None:
        return None
    
    queue = deque([root])  # Initialize the queue with the root
    
    while queue:
        current = queue.popleft()  # Get the current node from the queue
        
        # Swap the left and right children
        current.left, current.right = current.right, current.left
        
        # Add children to the queue if they are not None
        if current.left:
            queue.append(current.left)
        if current.right:
            queue.append(current.right)
    
    return root
```

**Time Complexity:** O(n), as we visit each node exactly once.

**Space Complexity:** O(n), where n is the number of nodes in the tree. In the worst case, the size of the queue could be n/2.

---

### Iterative Solution using DFS with Stack

Another iterative approach is to use a stack to perform a depth-first traversal.

**Intuition:**

1. Use a stack to process nodes in a depth order.
2. For each node, swap its left and right children.
3. Push the non-null left and right children onto the stack for further processing.

**Algorithm:**

- Initialize a stack and push the root node onto it.
- While the stack is not empty:
  - Pop a node from the stack.
  - Swap its left and right children.
  - Push non-null children onto the stack.

**Implementation:**

```python
def invertTreeIterativeDFS(root: TreeNode) -> TreeNode:
    if root is None:
        return None
    
    stack = [root]  # Initialize stack with the root
    
    while stack:
        current = stack.pop()  # Pop a node from the stack
        
        # Swap the left and right children
        current.left, current.right = current.right, current.left
        
        # Push non-null children onto the stack
        if current.left:
            stack.append(current.left)
        if current.right:
            stack.append(current.right)
    
    return root
```

**Time Complexity:** O(n), as every node is visited once.

**Space Complexity:** O(h), where h is the height of the tree. In the worst case, the space used by the stack could be O(n) if the tree is fully skewed.

