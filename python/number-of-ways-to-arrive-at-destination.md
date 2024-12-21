## [Leetcode 1976: Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

### Approaches:

- [Approach 1: Dijkstra's Algorithm with Priority Queue](#approach-1-dijkstras-algorithm-with-priority-queue)
- [Approach 2: Dijkstra's Algorithm with Dynamic Programming](#approach-2-dijkstras-algorithm-with-dynamic-programming)

---

### Approach 1: Dijkstra's Algorithm with Priority Queue

#### Intuition:
The problem is essentially about finding the shortest path in a weighted graph (road network) and counting the number of ways to achieve this shortest path from node 0 to node n-1. Dijkstra's algorithm is suitable for this, as it enables us to find the shortest paths from a source node to all other nodes in a graph with non-negative weights efficiently.

We can use Dijkstra's algorithm in combination with a priority queue to keep track of the minimal distances and count the number of ways to arrive at each node with those minimal distances.

#### Steps:
1. **Data Structures**:
   - Use a priority queue to efficiently get the node with the smallest tentative distance.
   - Maintain two lists: `dist` to store the shortest known distance to each node, and `ways` to store the number of ways to reach each node with the shortest distance.

2. **Initialization**:
   - Set the distance to the source node 0 to 0 and the number of ways to reach node 0 as 1.

3. **Algorithm Execution**:
   - While there are nodes to process in the priority queue:
     - Extract the node with the smallest distance.
     - For each neighbor node, calculate the potential new distance via the current node.
     - If this new distance is less than the known shortest distance, update the distance and reset the number of ways.
     - If the distance is equal to the known shortest distance, increment the number of ways.

4. **Return**:
   - The number of ways to reach node n-1 with the shortest distance.

```python
from heapq import heappop, heappush
from collections import defaultdict

def countPaths(n, roads):
    graph = defaultdict(list)
    for u, v, time in roads:
        graph[u].append((v, time))
        graph[v].append((u, time))
    
    # Constants
    MOD = 10**9 + 7
    
    # Dijkstra setup
    dist = [float('inf')] * n
    ways = [0] * n
    dist[0] = 0
    ways[0] = 1
    heap = [(0, 0)]  # (distance, node)
    
    while heap:
        current_dist, u = heappop(heap)
        
        if current_dist > dist[u]:
            continue
        
        for v, time in graph[u]:
            alt = current_dist + time
            if alt < dist[v]:
                dist[v] = alt
                ways[v] = ways[u]
                heappush(heap, (alt, v))
            elif alt == dist[v]:
                ways[v] = (ways[v] + ways[u]) % MOD
                
    return ways[n - 1]

# Time Complexity: O(E log V) where E is the number of edges and V is the number of vertices.
# Space Complexity: O(V + E) to store the graph and paths.
```

### Approach 2: Dijkstra's Algorithm with Dynamic Programming

#### Intuition:
This approach resembles the first one but emphasizes using a combination of Dijkstra's algorithm with a dynamic programming perspective, focusing more on the update strategy for paths and distances while avoiding unnecessary paths recalculations by ensuring we only consider nodes with current optimal paths.

#### Additional Steps:
1. **Dynamic Programming Table**:
   - Maintain a dynamic programming table that monitors the state of paths and their respective counts while updating based on Dijkstra’s priority order.

2. **Progressive Update**:
   - Update path counts progressively, ensuring that each node's state is resolved before it influences others, limiting recalculation.

The approach shares a similar solution code to the first one due to its inherent efficiency and optimal setup with Dijkstra's method.

```python
def countPaths(n, roads):
    graph = defaultdict(list)
    for u, v, time in roads:
        graph[u].append((v, time))
        graph[v].append((u, time))
    
    # Constants
    MOD = 10**9 + 7
    
    # Dijkstra setup
    dist = [float('inf')] * n
    ways = [0] * n
    dist[0] = 0
    ways[0] = 1
    heap = [(0, 0)]  # (distance, node)
    
    while heap:
        current_dist, u = heappop(heap)
        
        if current_dist > dist[u]:
            continue
        
        for v, time in graph[u]:
            alt = current_dist + time
            if alt < dist[v]:
                dist[v] = alt
                ways[v] = ways[u]
                heappush(heap, (alt, v))
            elif alt == dist[v]:
                ways[v] = (ways[v] + ways[u]) % MOD
                
    return ways[n - 1]

# Time Complexity: O(E log V) where E is the number of edges and V is the number of vertices.
# Space Complexity: O(V + E) to store the graph and paths.
```

Both approaches effectively handle the task of finding the number of ways to reach the destination node with the shortest possible path using efficient graph traversal enhanced by priority queue operations. By maintaining dynamically updated structures, they ensure the shortest paths are optimally quantified along with their respective counts.

