# [Leetcode 743: Network Delay Time](https://leetcode.com/problems/network-delay-time/)

## Approaches
1. [Breadth-First Search (BFS) using Queue](#approach-1-breadth-first-search-bfs-using-queue)
2. [Dijkstra’s Algorithm using Min-Heap](#approach-2-dijkstra-algorithm-using-min-heap)

## Approach 1: Breadth-First Search (BFS) using Queue

### Intuition
The problem requires finding the time taken for all nodes to receive a signal starting from a given node. The signal spreads like a wave, making BFS a suitable choice as it explores distance from the start node layer by layer. Here, we can incrementally update the shortest time for each node if we find a shorter path through our exploration.

### Steps
1. **Graph Representation**: Use an adjacency list to represent the graph. Each node points to other nodes it can directly connect to with the corresponding time taken.
2. **Initialization**: Use a dictionary to maintain the shortest time to reach each node, initializing with infinity (`float('inf')`) except for the starting node `K` which is `0`.
3. **Queue Setup**: Start BFS using a queue initialized with the starting node and accumulated time (0 for the starting node).
4. **Queue Processing**:
   - For each node, explore its neighbors.
   - If the total time to reach a neighbor is shorter than the previously recorded time, update it and enqueue the neighbor for further exploration.
5. **Result Computation**: After the BFS completes, check the maximum time from the shortest time dictionary. If any node has unreachable time (`float('inf')`), not all nodes can receive the signal.

### Code

```python
from collections import defaultdict, deque

def networkDelayTime(times, N, K):
    # Step 1: Create graph - adjacency list
    graph = defaultdict(list)
    for u, v, w in times:
        graph[u].append((v, w))

    # Step 2: Initialization
    shortest = {i: float('inf') for i in range(1, N + 1)}
    shortest[K] = 0

    # Step 3: BFS using queue
    queue = deque([(K, 0)])
    while queue:
        node, elapsed_time = queue.popleft()
        for neighbor, time in graph[node]:
            new_time = elapsed_time + time
            # Step 4: Update and enqueue
            if new_time < shortest[neighbor]:
                shortest[neighbor] = new_time
                queue.append((neighbor, new_time))

    # Step 5: Result computation
    result = max(shortest.values())
    return result if result < float('inf') else -1
```

### Complexity Analysis
- **Time Complexity**: \(O(N + E)\), where \(N\) is the number of nodes and \(E\) is the number of edges, since each node and edge is processed once.
- **Space Complexity**: \(O(N + E)\) for the graph representation and queue.

## Approach 2: Dijkstra’s Algorithm using Min-Heap

### Intuition
This problem is a classic shortest path problem that can be solved efficiently using Dijkstra's algorithm. Here, we make use of a priority queue (min-heap) to always expand the least costly node, ensuring optimal path calculation for nodes in increasing order of shortest path.

### Steps
1. **Graph Representation**: Again, use an adjacency list similar to the BFS.
2. **Initialization**: Maintain a dictionary for shortest times initialized to infinity except the starting node `K`. Additionally, use a set to track visited nodes.
3. **Min-Heap Setup**: Initially push the start node into the min-heap with a time of 0.
4. **Processing**:
   - Pop the node with the lowest accumulated time from the heap.
   - If already visited, continue; if not, mark it as visited.
   - Update the neighboring nodes' times if a shorter path is found and push these into the heap.
5. **Result Computation**: Determine and check the maximum time from the shortest time list for unreachable nodes.

### Code

```python
import heapq
from collections import defaultdict

def networkDelayTime(times, N, K):
    # Step 1: Create graph - adjacency list
    graph = defaultdict(list)
    for u, v, w in times:
        graph[u].append((v, w))
    
    # Step 2: Initialization
    shortest = {i: float('inf') for i in range(1, N + 1)}
    shortest[K] = 0
    visited = set()

    # Step 3: Use min-heap (priority queue)
    min_heap = [(0, K)]
    while min_heap:
        elapsed_time, node = heapq.heappop(min_heap)
        if node in visited:
            continue
        visited.add(node)
        # Step 4: Update and push neighbors
        for neighbor, time in graph[node]:
            new_time = elapsed_time + time
            if new_time < shortest[neighbor]:
                shortest[neighbor] = new_time
                heapq.heappush(min_heap, (new_time, neighbor))

    # Step 5: Result computation
    result = max(shortest.values())
    return result if result < float('inf') else -1
```

### Complexity Analysis
- **Time Complexity**: \(O((N + E) \log N)\) due to edge relaxation operations and the heap operations where each potentially involves a \(\log N\) factor.
- **Space Complexity**: \(O(N + E)\) for storing the graph and min-heap.

