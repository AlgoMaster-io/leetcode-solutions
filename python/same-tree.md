# [Leetcode 100: Same Tree](https://leetcode.com/problems/same-tree/)

## Table of Contents
- [Approach 1: Recursive Depth-First Search (DFS)](#approach-1-recursive-dfs)
- [Approach 2: Iterative Depth-First Search Using Stack](#approach-2-iterative-dfs-using-stack)
- [Approach 3: Breadth-First Search Using Queue](#approach-3-breadth-first-search-using-queue)

## Approach 1: Recursive Depth-First Search (DFS)

### Intuition
The idea is to leverage recursion to compare the current nodes of both trees and, if they are identical, recursively check the left and right subtrees. If both nodes being compared are `None`, they are trivially the same. If any one of them is `None`, or their values differ, the trees are not the same.

### Algorithm
1. Start the recursion with the root nodes of both trees.
2. If both nodes are `None`, trees are identical up to this point, return `True`.
3. If one node is `None` and the other is not, or if their values differ, return `False`.
4. Recursively check the left and right children.
5. Return `True` only if both subtrees are identical.

### Python Code
```python
def isSameTree(p: TreeNode, q: TreeNode) -> bool:
    # If both nodes are None, they are identical
    if not p and not q:
        return True
    
    # If one of them is None, or values don't match, trees are not identical
    if not p or not q or p.val != q.val:
        return False
    
    # Recursively check left and right subtrees
    return isSameTree(p.left, q.left) and isSameTree(p.right, q.right)
```

### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the tree, since each node is visited once.
- **Space Complexity**: O(H), where H is the height of the tree, accounting for recursion stack space.

## Approach 2: Iterative Depth-First Search Using Stack

### Intuition
Instead of recursion, we use an explicit stack to perform DFS. The stack will hold pairs of nodes, starting with the roots of both trees. We process node pairs one at a time.

### Algorithm
1. Use a stack initialized with the root nodes of both trees.
2. While the stack is not empty:
   - Pop a node pair from the stack.
   - If both nodes are `None`, continue to next iteration.
   - If one node is `None` or their values differ, return `False`.
   - Push children of both nodes onto the stack (both left and right).
3. If all nodes are processed without mismatches, return `True`.

### Python Code
```python
def isSameTree(p: TreeNode, q: TreeNode) -> bool:
    stack = [(p, q)]
    
    while stack:
        node_p, node_q = stack.pop()
        
        # If both nodes are None, identical at this level, continue
        if not node_p and not node_q:
            continue
        
        # If one is None or values don't match, trees aren't the same
        if not node_p or not node_q or node_p.val != node_q.val:
            return False
        
        # Push corresponding children onto the stack
        stack.append((node_p.right, node_q.right))
        stack.append((node_p.left, node_q.left))
    
    return True
```

### Complexity Analysis
- **Time Complexity**: O(N), each node is processed once.
- **Space Complexity**: O(H), due to stack space usage, where H is the height of the tree.

## Approach 3: Breadth-First Search Using Queue

### Intuition
Utilize a queue to process nodes level by level. This ensures each level is completely compared before moving onto the next.

### Algorithm
1. Initialize a queue with the root nodes of both trees.
2. While the queue is not empty:
   - Dequeue a pair of nodes.
   - If both nodes are `None`, continue.
   - If values differ or only one is `None`, return `False`.
   - Enqueue children of both nodes.
3. If all node pairs match, return `True`.

### Python Code
```python
from collections import deque

def isSameTree(p: TreeNode, q: TreeNode) -> bool:
    queue = deque([(p, q)])
    
    while queue:
        node_p, node_q = queue.popleft()
        
        # Both are None, continue to next level
        if not node_p and not node_q:
            continue
        
        # Mismatched nodes, trees are not same
        if not node_p or not node_q or node_p.val != node_q.val:
            return False
        
        # Enqueue children nodes for further comparison
        queue.append((node_p.left, node_q.left))
        queue.append((node_p.right, node_q.right))
    
    return True
```

### Complexity Analysis
- **Time Complexity**: O(N), each node is processed.
- **Space Complexity**: O(W), where W is the maximum width of the trees (most nodes at any level), due to queue usage.

