# 1976. [Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

## Approach 1: Dijkstra's Algorithm with Modification

### Solution
python
```python
# Time Complexity: O(E log V) where E is the number of edges and V is the number of vertices
# Space Complexity: O(V)
import heapq

class Solution:
    def countPaths(self, n: int, roads: List[List[int]]) -> int:
        MOD = 1_000_000_007
        graph = [[] for _ in range(n)]
        
        # Construct the graph
        for u, v, time in roads:
            graph[u].append((v, time))
            graph[v].append((u, time))

        # Dijkstra's algorithm modifications
        minDist = [float('inf')] * n
        ways = [0] * n
        ways[0] = 1
        minDist[0] = 0

        pq = [(0, 0)]  # (distance, node)

        while pq:
            currentDist, u = heapq.heappop(pq)

            if currentDist > minDist[u]:
                continue

            # Traverse neighbors
            for v, time in graph[u]:
                # Relaxation
                if minDist[u] + time < minDist[v]:
                    minDist[v] = minDist[u] + time
                    ways[v] = ways[u]
                    heapq.heappush(pq, (minDist[v], v))
                elif minDist[u] + time == minDist[v]:
                    ways[v] = (ways[v] + ways[u]) % MOD

        return ways[n - 1]
```

This solution efficiently finds the number of shortest paths from node 0 to node n-1 using a modified version of Dijkstra's algorithm that keeps track of the number of ways to arrive at each node. It uses a priority queue to explore the shortest paths and maintains an array to store the number of ways to reach each node at the shortest distance.

