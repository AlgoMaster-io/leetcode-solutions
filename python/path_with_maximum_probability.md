# 1514. [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approach 1: Dijkstra's Algorithm using a Priority Queue

### Solution
python
```python
# Time Complexity: O((n + m) * log n), where n is the number of nodes and m is the number of edges
# Space Complexity: O(n + m)
import heapq
from collections import defaultdict

class Solution:
    def maxProbability(self, n, edges, succProb, start, end):
        graph = defaultdict(list)
        
        # Build adjacency list for the graph
        for (u, v), prob in zip(edges, succProb):
            graph[u].append((v, prob))
            graph[v].append((u, prob))  # Since the graph is undirected

        probabilities = [0.0] * n  # Store max probability to each node
        probabilities[start] = 1.0  # Max probability to start node is 1

        # Max-heap with negative probabilities as Python's heapq is a min-heap
        pq = [(-1.0, start)]

        # Process nodes with Dijkstra's algorithm
        while pq:
            currProb, currNode = heapq.heappop(pq)
            currProb = -currProb  # Convert back to positive

            # If we reach the end node, return probability
            if currNode == end:
                return currProb

            if currProb < probabilities[currNode]:
                continue

            for nextNode, edgeProb in graph[currNode]:
                # Calculate new probability
                newProb = currProb * edgeProb
                if newProb > probabilities[nextNode]:
                    probabilities[nextNode] = newProb
                    heapq.heappush(pq, (-newProb, nextNode))  # Push negative to maintain max-heap property

        return probabilities[end]  # Return max probability to reach end node
```

This problem is effectively a modification of the shortest path problem or Dijkstra's algorithm by changing distance sums to probabilities multiplied.

