# 787. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Approach 1: DFS with Pruning

### Solution
```python
# Time Complexity: O(n^k) in the worst case
# Space Complexity: O(n)
from collections import defaultdict

class Solution:
    def __init__(self):
        self.result = float('inf')

    def findCheapestPrice(self, n, flights, src, dst, K):
        # Create adjacency map for graph edges
        graph = defaultdict(dict)
        for u, v, price in flights:
            graph[u][v] = price

        # Perform DFS
        self.dfs(graph, src, dst, K + 1, 0)

        return -1 if self.result == float('inf') else self.result

    def dfs(self, graph, node, dst, stops, cost):
        if node == dst:
            self.result = cost
            return

        if stops == 0:
            return  # No more stops available

        if node not in graph:
            return  # No outgoing flights

        for neighbor, price in graph[node].items():
            # Pruning: proceed only if this path is cheaper than the best found so far
            if cost + price > self.result:
                continue

            self.dfs(graph, neighbor, dst, stops - 1, cost + price)
```

## Approach 2: Bellman-Ford Algorithm

### Solution
```python
# Time Complexity: O(K * E), where E is the number of edges
# Space Complexity: O(n)
import sys

class Solution:
    def findCheapestPrice(self, n, flights, src, dst, K):
        prices = [sys.maxsize] * n
        prices[src] = 0

        # Relax the edges up to K+1 times
        for _ in range(K + 1):
            temp_prices = prices[:]
            for u, v, price in flights:
                if prices[u] == sys.maxsize:
                    continue

                if prices[u] + price < temp_prices[v]:
                    temp_prices[v] = prices[u] + price
            prices = temp_prices

        return -1 if prices[dst] == sys.maxsize else prices[dst]
```

## Approach 3: Dijkstra's Algorithm with Priority Queue

### Solution
```python
# Time Complexity: O(E + VlogV), where E is the number of edges and V is the number of vertices
# Space Complexity: O(n)
import heapq
from collections import defaultdict

class Solution:
    def findCheapestPrice(self, n, flights, src, dst, K):
        # Create an adjacency list for the graph
        graph = defaultdict(list)
        for u, v, price in flights:
            graph[u].append((v, price))

        # Priority queue will hold entries of (cost, node, stops remaining)
        pq = [(0, src, K + 1)]

        while pq:
            cost, node, stops_remaining = heapq.heappop(pq)

            if node == dst:
                return cost

            if stops_remaining > 0:
                if node not in graph:
                    continue
                for next_node, price_to_next in graph[node]:
                    heapq.heappush(pq, (cost + price_to_next, next_node, stops_remaining - 1))

        return -1  # No valid path found
```

