# [Leetcode 102: Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## Approaches:
- [Approach 1: Breadth-First Search using Iterative Queue Approach](#approach-1-breadth-first-search-using-iterative-queue-approach)
- [Approach 2: Recursive Depth-First Search Approach](#approach-2-recursive-depth-first-search-approach)

---

### Approach 1: Breadth-First Search using Iterative Queue Approach

#### Intuition:
We can use a queue data structure to perform a level order traversal of the tree. The idea is to traverse the tree level by level, pushing the children of each node to the queue, and then processing these nodes until the queue is empty.

#### Steps:
1. Start by checking if the root is `None`. If it is, return an empty list.
2. Initialize a queue, starting with the root node.
3. While the queue is not empty, process nodes level by level:
   - Determine the number of nodes at the current level (`level_size`).
   - Iterate over the number of nodes in the current level and pop nodes from the queue, collecting their values.
   - Add the left and right children of each node to the queue.
4. Keep track of values node-wise and then append level-wise nodes list to result.
5. Return the list containing node values organized in level order.

```python
from collections import deque

def levelOrder(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        current_level = []
        
        for _ in range(level_size):
            node = queue.popleft() # Dequeue a node from the queue
            current_level.append(node.val) # Collect its value
            
            # Enqueue the left child if it exists
            if node.left:
                queue.append(node.left)
            
            # Enqueue the right child if it exists
            if node.right:
                queue.append(node.right)
        
        result.append(current_level) # Append the current level to the result
    
    return result
```
- **Time Complexity**: O(N), where N is the number of nodes in the tree because we visit each node exactly once.
- **Space Complexity**: O(N), as we store node values at each level (worst case if the tree is completely unbalanced).

---

### Approach 2: Recursive Depth-First Search Approach

#### Intuition:
This approach involves using recursion to traverse the tree and maintain the state of each level. By passing the current level as a parameter during recursive calls, the method distinguishes nodes belonging to each level.

#### Steps:
1. Define a helper function that takes the node and the current depth/level.
2. Base case: If the node is `None`, return.
3. If the current depth equals the size of the result list, this means you're visiting a new depth level, so append a new list for it.
4. Append the current node's value to the corresponding list in the result.
5. Recursively call the helper function on the left and right children, incrementing the level.
6. Invoke the helper function starting with the tree's root and level 0. Return the result list.

```python
def levelOrder(root):
    def dfs(node, level):
        if not node:
            return
        
        # Check if a new level has been reached
        if level == len(result):
            result.append([]) # Start a new list for this level
        
        result[level].append(node.val) # Add the current node's value to its level
        
        dfs(node.left, level + 1)  # Traverse the left subtree
        dfs(node.right, level + 1) # Traverse the right subtree
    
    result = []
    dfs(root, 0)
    return result
```
- **Time Complexity**: O(N), where N is the number of nodes in the tree because each node is visited once.
- **Space Complexity**: O(H), where H is the height of the tree, in terms of maximum function call stack (recursive stack space).

Each approach provides a clear methodology for performing a level order traversal of a binary tree, utilizing different traversal techniques for flexibility and understanding.

