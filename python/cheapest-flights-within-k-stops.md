# [Leetcode 787: Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Solutions
- [Breadth-First Search with Queue](#approach-1-breadth-first-search-with-queue)
- [Dynamic Programming with Bellman-Ford](#approach-2-dynamic-programming-with-bellman-ford)
- [Dijkstra's Algorithm](#approach-3-dijkstras-algorithm)

### Approach 1: Breadth-First Search with Queue

#### Intuition
We will employ a breadth-first search (BFS) starting from the source node. Instead of the common BFS approach, we will take into account the cost of reaching each node. Utilizing a queue, we maintain the current position, current cost, and the number of stops. Our primary aim is to reach the destination with the least cost while accounting for <= K stops.

#### Steps:
1. We will use a priority queue (min-heap), where each entry is a tuple (cost, current_city, remaining stops).
2. Initialize the heap with the starting node with cost 0 and K+1 stops available (to allow source as stop 0).
3. Continue exploring nodes in the queue:
   - If the node is the destination, return the current cost.
   - For each neighboring city, calculate the cost to reach from the current node and add to the queue if stops are available.
4. If no valid path is found within K stops, return -1.

#### Code
```python
import collections
import heapq

def findCheapestPrice(n, flights, src, dst, K):
    # Build adjacency list representation of the graph
    graph = collections.defaultdict(list)
    for u, v, w in flights:
        graph[u].append((v, w))
    
    # Priority queue for minimum cost expanded search
    queue = [(0, src, K + 1)]  # Cost, Node, Stops remaining

    while queue:
        cost, node, stops = heapq.heappop(queue)
        
        # If we've reached the destination, return the cost
        if node == dst:
            return cost
        
        # If stops are remaining, check the neighbors
        if stops > 0:
            for v, w in graph[node]:
                heapq.heappush(queue, (cost + w, v, stops - 1))
                
    return -1
```

#### Complexity Analysis
- **Time Complexity:** \(O(n \cdot \log n + E)\), where n is the number of nodes (cities) and E is the number of edges (flights), because we could potentially explore each city once and process its neighbors.
- **Space Complexity:** \(O(n + E)\), for the graph representation and queue storage.

---

### Approach 2: Dynamic Programming with Bellman-Ford

#### Intuition
We will apply dynamic programming with the Bellman-Ford algorithm to solve this problem by keeping track of the minimum cost to reach each node with up to K stops. We will update our minimum cost iteratively for each round of allowable flights.

#### Steps:
1. Initialize a distance array with infinity for each node, and set the cost to reach the source to 0.
2. For K+1 iterations, attempt to relax all edges:
   - Create a copy of the distance array to avoid premature updates.
   - If a valid transition through an edge results in a lower cost, update it in the copy.
3. The result will be the cost to reach the destination node, or -1 if unreachable within K stops.

#### Code
```python
def findCheapestPrice(n, flights, src, dst, K):
    # Initialize the distance array with infinity
    distances = [float('inf')] * n
    distances[src] = 0

    # Relax edges up to K + 1 times
    for _ in range(K + 1):
        # Copy of the current distances to use in updating
        new_distances = distances[:]

        for u, v, w in flights:
            if distances[u] != float('inf') and distances[u] + w < new_distances[v]:
                new_distances[v] = distances[u] + w
                
        # Move to the next iteration
        distances = new_distances
    
    return -1 if distances[dst] == float('inf') else distances[dst]
```

#### Complexity Analysis
- **Time Complexity:** \(O(K \cdot E)\), where K is the number of stops allowed and E is the number of flights.
- **Space Complexity:** \(O(n)\), for storing distances.

---

### Approach 3: Dijkstra's Algorithm

#### Intuition
Leverage a priority queue to efficiently find the minimum cost to reach a node. The Dijkstra's algorithm is used here with a slight modification to account for the number of stops.

#### Steps:
1. Maintain a min-heap, each entry containing (current cost, current city, remaining stops).
2. Use a priority queue to always dequeue the path with the least cost first.
3. If the destination city is reached within the allowed stops, return the cost.
4. Continue to explore from the current city:
   - Calculate the next step cost and push into the heap with the remaining stops reduced by one.
5. Return -1 if no path to destination is found within K stops.

#### Code
```python
import heapq

def findCheapestPrice(n, flights, src, dst, K):
    graph = collections.defaultdict(list)
    for u, v, w in flights:
        graph[u].append((v, w))
    
    # Min-heap for BFS, storing (cost, node, stops remaining)
    heap = [(0, src, K + 1)]
    
    while heap:
        cost, node, stops = heapq.heappop(heap)
        
        # If we have reached the destination city
        if node == dst:
            return cost
        
        # If there are stops left, explore neighbors
        if stops > 0:
            for v, w in graph[node]:
                heapq.heappush(heap, (cost + w, v, stops - 1))
                
    return -1
```

#### Complexity Analysis
- **Time Complexity:** \(O(E \log n)\), where E is the number of flights.
- **Space Complexity:** \(O(n + E)\), due to storing graph representation and heap.

