# [Clone Graph - LeetCode](https://leetcode.com/problems/clone-graph/)

## Approaches

- [Approach 1: Depth-First Search (DFS) with Recursion](#approach-1-depth-first-search-dfs-with-recursion)
- [Approach 2: Breadth-First Search (BFS) with Iteration](#approach-2-breadth-first-search-bfs-with-iteration)
- [Approach 3: Depth-First Search (DFS) with Iteration](#approach-3-depth-first-search-dfs-with-iteration)

## Approach 1: Depth-First Search (DFS) with Recursion

### Intuition
The DFS approach leverages the recursive nature to traverse the graph and clone each node. This approach uses a dictionary to keep track of all cloned nodes, mapping each original node to its clone to avoid redundant work and handle cycles.

### Algorithm
1. If the input node is null, return null.
2. Check if the node is already cloned by using a hashmap. If yes, return the cloned node from the hashmap.
3. If not cloned, create a new node with the same value as the original node.
4. Store the cloned node in the hashmap using the original node as the key.
5. Recursively call the function for all adjacent nodes (neighbors) and attach the cloned neighbors to the corresponding cloned node’s neighbors list.
6. Finally, return the cloned start node.

### Time Complexity
- **Time:** O(N), where N is the number of nodes. Each node is visited once.
- **Space:** O(N), for the recursion stack and hash map storing the visited nodes.

### Code
```python
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

def cloneGraph(node: 'Node') -> 'Node':
    if not node:
        return None
    
    # This hashmap will store the cloned nodes
    cloned_nodes = {}

    # Recursive function to perform DFS clone
    def dfs(node):
        # If this node is already cloned, return its clone
        if node in cloned_nodes:
            return cloned_nodes[node]

        # Clone the node
        clone = Node(node.val)
        # Add it to the hashmap
        cloned_nodes[node] = clone

        # Iterate over the neighbors to clone them
        for neighbor in node.neighbors:
            # Append cloned neighbor nodes to the neighbor list of the clone node
            clone.neighbors.append(dfs(neighbor))
        
        return clone

    # Start DFS traversal and cloning from the input node
    return dfs(node)
```

## Approach 2: Breadth-First Search (BFS) with Iteration

### Intuition
This approach uses a queue to perform BFS traversal iteratively. It maintains a hashmap to map each original node to its clone, ensuring each node is cloned exactly once. BFS is particularly useful for level-order traversal of the graph.

### Algorithm
1. If the input node is null, return null.
2. Initialize a queue for BFS and a hashmap to store cloned nodes.
3. Enqueue the starting node and create its clone.
4. For each node:
    - Dequeue from the queue.
    - For each neighbor of the node:
        - If the neighbor is not cloned, clone it, store it in the hashmap, and enqueue it.
        - Add the cloned neighbor to the neighbors list of the current node's clone.
5. Return the clone of the starting node.

### Time Complexity
- **Time:** O(N), where N is the number of nodes.
- **Space:** O(N), for the queue and hash map storing the cloned nodes.

### Code
```python
from collections import deque

def cloneGraph(node: 'Node') -> 'Node':
    if not node:
        return None
    
    # This map will hold the cloned nodes
    cloned_nodes = {}
    
    # Initialize BFS queue
    queue = deque([node])
    
    # Clone the starting node
    cloned_nodes[node] = Node(node.val)

    # BFS Traversal
    while queue:
        current = queue.popleft()
        
        # Iterate through each neighbor of the current node
        for neighbor in current.neighbors:
            if neighbor not in cloned_nodes:
                # Clone the neighbor and put it in the hashmap
                cloned_nodes[neighbor] = Node(neighbor.val)
                # Append newly cloned neighbor node to the BFS queue
                queue.append(neighbor)
            
            # Append the cloned neighbor to the current node's clone's neighbors list
            cloned_nodes[current].neighbors.append(cloned_nodes[neighbor])
    
    # Return the cloned graph starting node
    return cloned_nodes[node]
```

## Approach 3: Depth-First Search (DFS) with Iteration

### Intuition
This approach also utilizes DFS but does so iteratively using a stack. It’s similar in operation to recursive DFS but manually manages the stack structure to avoid recursion.

### Algorithm
1. If the input node is null, return null.
2. Create a stack for DFS and a hashmap to store cloned nodes.
3. Clone the input node and push it onto the stack.
4. While the stack is not empty:
    - Pop the node.
    - Traverse its neighbors:
        - If a neighbor is not cloned, clone it, add it to the hashmap, and push it onto the stack.
        - Add the cloned neighbor to the current node's clone’s neighbors list.
5. Return the clone of the input node.

### Time Complexity
- **Time:** O(N), where N is the number of nodes.
- **Space:** O(N), for the stack and hash map storing the cloned nodes.

### Code
```python
def cloneGraph(node: 'Node') -> 'Node':
    if not node:
        return None
    
    # Hashmap to store cloned nodes
    cloned_nodes = {}
    # Stack for DFS
    stack = [node]
    # Initialize the clone of the initial node
    cloned_nodes[node] = Node(node.val)
    
    # Iterative DFS traversal
    while stack:
        current = stack.pop()
        
        for neighbor in current.neighbors:
            if neighbor not in cloned_nodes:
                # If neighbor is not cloned, clone and add to stack
                cloned_nodes[neighbor] = Node(neighbor.val)
                stack.append(neighbor)
            
            # Append the neighbor's clone to the current node clone's neighbors list
            cloned_nodes[current].neighbors.append(cloned_nodes[neighbor])
    
    # Return the clone of the input node
    return cloned_nodes[node]
```

Each method provides a different perspective on solving graph traversal and copying problems, and it's always insightful to evaluate them based on the specific needs of the problem context, including constraints like space and recursion depth.

