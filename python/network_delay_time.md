# 743. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
python
```python
# Time Complexity: O((V + E) log V)
# Space Complexity: O(V + E)
import heapq
from collections import defaultdict

class Solution:
    def networkDelayTime(self, times, n, k):
        # Create a graph as an adjacency list representation
        graph = defaultdict(list)
        for u, v, w in times:
            graph[u].append((v, w))
        
        # Priority Queue to hold nodes as pairs of (time, node)
        pq = [(0, k)]
        
        # Dictionary to track the minimum time to reach each node
        min_time_to_reach = {}
        
        # Dijkstra's algorithm
        while pq:
            current_time, current_node = heapq.heappop(pq)
            if current_node in min_time_to_reach:
                continue
            min_time_to_reach[current_node] = current_time
            for next_node, travel_time in graph[current_node]:
                if next_node not in min_time_to_reach:
                    heapq.heappush(pq, (current_time + travel_time, next_node))
        
        # Determine the maximum time needed
        if len(min_time_to_reach) != n:
            return -1
        
        return max(min_time_to_reach.values())
```

## Approach 2: Bellman-Ford Algorithm

### Solution
python
```python
# Time Complexity: O(V * E)
# Space Complexity: O(V)
class Solution:
    def networkDelayTime(self, times, n, k):
        dist = [float('inf')] * (n + 1)
        dist[k] = 0
        
        # Relaxation process for V-1 times
        for _ in range(n - 1):
            for u, v, w in times:
                if dist[u] != float('inf') and dist[u] + w < dist[v]:
                    dist[v] = dist[u] + w
        
        max_time = 0
        for i in range(1, n + 1):
            if dist[i] == float('inf'):
                return -1
            max_time = max(max_time, dist[i])
        
        return max_time
```

## Approach 3: Dynamic Programming with Floyd-Warshall Algorithm

### Solution
python
```python
# Time Complexity: O(V^3)
# Space Complexity: O(V^2)
class Solution:
    def networkDelayTime(self, times, n, k):
        dp = [[float('inf')] * (n + 1) for _ in range(n + 1)]
        for i in range(n + 1):
            dp[i][i] = 0 # Distance from a node to itself is 0
        
        # Initialize graph with given times
        for u, v, w in times:
            dp[u][v] = w
        
        # Floyd-Warshall algorithm for shortest paths
        for t in range(1, n + 1):
            for u in range(1, n + 1):
                for v in range(1, n + 1):
                    dp[u][v] = min(dp[u][v], dp[u][t] + dp[t][v])
        
        # Calculate the longest shortest path from the start node k
        max_time = 0
        for i in range(1, n + 1):
            if dp[k][i] == float('inf'):
                return -1
            max_time = max(max_time, dp[k][i])
        
        return max_time
```

