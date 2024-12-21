# [Leetcode 103: Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

## Approaches:
1. [Breadth-First Search (BFS) with Level Tracking](#approach-1-bfs-with-level-tracking)
2. [Breadth-First Search with Zigzag Toggle](#approach-2-bfs-with-zigzag-toggle)

## Approach 1: BFS with Level Tracking

### Intuition:
The problem requires us to traverse the binary tree level by level but in a zigzag manner. The simplest way to achieve level order traversal is through Breadth-First Search (BFS). We'll use a queue to facilitate the level order traversal but maintain a flag or mechanism to switch the order of node collection for alternate levels.

### Steps:
- Use a queue to hold nodes at each level.
- Track the current depth to determine whether we should append nodes left-to-right or right-to-left.
- Use a list to store the result of each level in the required order.
- At each level, process nodes and enqueue their children.
- Reverse the list of nodes for the current level based on the zigzag requirement.

### Code:

```python
from collections import deque

def zigzagLevelOrder(root):
    if not root:
        return []
    
    results = []
    nodes_queue = deque([root])
    left_to_right = True
    
    while nodes_queue:
        level_size = len(nodes_queue)
        level_nodes = []
        
        for _ in range(level_size):
            node = nodes_queue.popleft()
            # Append the node's value to the level list
            level_nodes.append(node.val)
            # Enqueue left child if available
            if node.left:
                nodes_queue.append(node.left)
            # Enqueue right child if available
            if node.right:
                nodes_queue.append(node.right)
        
        # If we need to reverse this level's nodes
        if not left_to_right:
            level_nodes.reverse()
        
        # Append the level's result to the final result list
        results.append(level_nodes)
        # Toggle direction for the next level
        left_to_right = not left_to_right
    
    return results
```

### Time Complexity:
- O(N), where N is the number of nodes in the tree, since each node is visited exactly once.

### Space Complexity:
- O(N), due to the space used by the queue to store nodes at each level.

## Approach 2: BFS with Zigzag Toggle

### Intuition:
This approach is a variation on the first BFS approach. Instead of reversing the list of level nodes, we can alternate the order of appending elements directly using deque operations. This utilizes the deque to mimic stack operations when needed, providing an in-place modification of the current level order.

### Steps:
- Again, employ a queue to manage nodes.
- Alter the mechanism to append nodes to a deque rather than a list based on the level order.
- Utilize `appendleft` for reverse order and `append` for normal order based on the zigzag condition.
- Convert the deque back to a list and store it in results.

### Code:

```python
from collections import deque

def zigzagLevelOrder(root):
    if not root:
        return []
    
    results = []
    nodes_queue = deque([root])
    left_to_right = True
    
    while nodes_queue:
        level_size = len(nodes_queue)
        level_nodes = deque()  # Use a deque to facilitate zigzag order at each level
        
        for _ in range(level_size):
            node = nodes_queue.popleft()
            
            if left_to_right:
                # Add node value to the right if the level is left to right
                level_nodes.append(node.val)
            else:
                # Add node value to the left if the level is right to left
                level_nodes.appendleft(node.val)
            
            # Enqueue child nodes for the next level processing
            if node.left:
                nodes_queue.append(node.left)
            if node.right:
                nodes_queue.append(node.right)
        
        # Append the current level's deque as a list to the results.
        results.append(list(level_nodes))
        # Toggle the direction for next level processing
        left_to_right = not left_to_right
    
    return results
```

### Time Complexity:
- O(N), as each node is still visited exactly once.

### Space Complexity:
- O(N), additional space complexity from using deque to handle each level's nodes.

