# [Leetcode 101: Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Iterative Approach Using Queue](#iterative-approach-using-queue)

---

## Recursive Approach

### Intuition
The core idea is to realize that a binary tree is symmetric if the left subtree is a mirror reflection of the right subtree. Thus, we can recursively check if the left and right subtrees are mirrors of each other. Starting from the root, we should:
1. Compare the two nodes (initially left and right children of the root).
2. They should be equal and the subtrees of these nodes should also be mirrors:
   - The left child of the left node should be a mirror of the right child of the right node.
   - The right child of the left node should be a mirror of the left child of the right node.

### Implementation

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def isSymmetric(root: TreeNode) -> bool:
    # Helper function to check if two trees are mirror images of each other
    def isMirror(left: TreeNode, right: TreeNode) -> bool:
        # Base cases
        if not left and not right:
            return True  # Both are null, hence symmetric
        if not left or not right:
            return False  # One is null, the other isn't, hence not symmetric
        
        # Check values and recursively check their respective children
        return (left.val == right.val) and \
               isMirror(left.left, right.right) and \
               isMirror(left.right, right.left)

    return isMirror(root.left, root.right) if root else True
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the number of nodes in the tree. Each node is visited once.
- **Space Complexity:** O(h), where h is the height of the tree. This space is used for the recursion stack.

---

## Iterative Approach Using Queue

### Intuition
Instead of using recursion, we can simulate it using a queue. We'll use two queues to store pairs of nodes from the left and right subtrees that should be compared against each other. The idea is to continuously dequeue front elements (nodes) from both queues and:
1. Check if both are null, continue if true.
2. If one is null while the other isn't, the tree isn't symmetric.
3. If both nodes are non-null, their values should be equal, and their children should be enqueued in opposite order to continue the mirror check.

### Implementation

```python
from collections import deque

def isSymmetric(root: TreeNode) -> bool:
    if not root:
        return True

    # Initialize a queue to store pairs of nodes
    queue = deque([(root.left, root.right)])
    
    while queue:
        left, right = queue.popleft()
        
        # Both null means symmetric at this level
        if not left and not right:
            continue
        
        # One null, one not means not symmetric
        if not left or not right:
            return False
        
        # Values must be the same
        if left.val != right.val:
            return False

        # Enqueue children nodes for further comparison
        queue.append((left.left, right.right))
        queue.append((left.right, right.left))
    
    # If all comparisons are symmetric
    return True
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the number of nodes in the tree. Each node is visited once.
- **Space Complexity:** O(n), where n is the number of nodes in the tree. In the worst case, the queue can store all nodes at a single level of the tree.

---

