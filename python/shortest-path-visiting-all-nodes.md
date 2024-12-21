# [Leetcode 847: Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

## Approaches
- [Approach 1: Breadth-First Search (BFS) with Bitmasking](#approach-1)
- [Approach 2: Optimized BFS with visited state](#approach-2)

## Approach 1: Breadth-First Search (BFS) with Bitmasking

### Intuition
The problem is essentially a shortest path problem through a graph, requiring us to visit all nodes. Given the constraints (n <= 12), a BFS approach is viable. To efficiently mark "visited" nodes, we can use a bitmask, where each bit in an integer represents whether a corresponding node has been visited. This enables us to manage multiple states compactly.

### Steps
1. We start BFS from every node since any node could potentially be the starting point for the shortest path.
2. We use a queue to store tuples of the current node, the bitmask representing visited nodes, and the current step count.
3. A set keeps track of visited (node, bitmask) pairs to prevent revisiting the same state.
4. We iterate over the graph using BFS, updating the bitmask as we visit each node.
5. If the bitmask indicates all nodes have been visited (all bits set), return the step count.
6. If a new (node, bitmask) combination is encountered, add it to the queue for further exploration.

### Code

```python
from collections import deque

def shortestPathLength(graph):
    n = len(graph)
    # All nodes visited bitmask target = (1 << n) - 1, e.g., for 4 nodes 1111 = 15
    target = (1 << n) - 1
    
    # Queue for BFS: each element is (cur_node, cur_mask, steps)
    queue = deque()
    # Visited set to avoid revisiting same state
    visited = set()
    
    # Initialize queue with all possible starting nodes
    for i in range(n):
        # Each node in separate bitmask initially
        state = (i, 1 << i, 0)  # (node, visited_mask, steps)
        queue.append(state)
        visited.add((i, 1 << i))
    
    while queue:
        current_node, current_mask, steps = queue.popleft()
        
        # Check if all nodes have been visited
        if current_mask == target:
            return steps
        
        # Explore neighbors
        for neighbor in graph[current_node]:
            next_mask = current_mask | (1 << neighbor)  # Set the bit for the neighbor node
            if (neighbor, next_mask) not in visited:
                visited.add((neighbor, next_mask))
                queue.append((neighbor, next_mask, steps + 1))
    
    return -1  # Just in case, we should never hit this due to problem constraints

```

### Time Complexity
- **O(2^n * n^2)**: For each of the 2^n possible visited states, there are at most n nodes and each with n neighbors.

### Space Complexity 
- **O(2^n * n)**: Keeping track of states and queue size.

---

## Approach 2: Optimized BFS with visited state

### Intuition
We can optimize the space further by reducing the number of visited states we keep track of. Instead of tracking all visited states using a set, we can prune states and reduce unnecessary storage use by carefully choosing when to add a state to the queue.

### Steps
1. The setup remains similar to the first approach with a queue and a bitmask.
2. Every time a state reaches a full bitmask, return the number of steps.
3. Reduce state space by only keeping necessary (node, mask) combinations.

### (This approach follows similar code to the first but focuses on instance optimization that might be redundant here; hence, the example is omitted.)

### Time Complexity
- Same as above due to state space complexity.

### Space Complexity
- Improved in practice but theoretically the same.

---

These solutions efficiently find the shortest path visiting all nodes using bitmasking and BFS, considering all graph paths from the possible initial nodes. The problem constraints make the bitmask approach feasible even with its exponential growth due to small `n`.

