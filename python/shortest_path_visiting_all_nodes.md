# 847. [Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

## Approach 1: Breadth-First Search (BFS) with Bitmask

### Solution
```python
# Time Complexity: O(2^n * n^2)
# Space Complexity: O(2^n * n)

from collections import deque

class Solution:
    def shortestPathLength(self, graph):
        n = len(graph)
        if n == 1:
            return 0

        # Queue for BFS, storing (node, visited nodes mask)
        queue = deque()
        visited = [[False] * (1 << n) for _ in range(n)]

        # Initialize queue and visited set with all starting nodes
        for i in range(n):
            queue.append((i, 1 << i))
            visited[i][1 << i] = True

        steps = 0
        while queue:
            size = len(queue)
            for _ in range(size):
                node, mask = queue.popleft()

                # If all nodes are visited
                if mask == (1 << n) - 1:
                    return steps

                # Visit all the neighbors
                for nei in graph[node]:
                    nextMask = mask | (1 << nei)
                    if not visited[nei][nextMask]:
                        queue.append((nei, nextMask))
                        visited[nei][nextMask] = True
            steps += 1
        return -1
```

## Approach 2: Dynamic Programming with Bitmask

### Solution
```python
# Time Complexity: O(2^n * n^2)
# Space Complexity: O(2^n * n)

from collections import deque

class Solution:
    def shortestPathLength(self, graph):
        n = len(graph)
        dp = [[float('inf')] * n for _ in range(1 << n)]

        queue = deque()
        for i in range(n):
            queue.append((1 << i, i))
            dp[1 << i][i] = 0

        while queue:
            mask, node = queue.popleft()

            for nei in graph[node]:
                nextMask = mask | (1 << nei)
                if dp[nextMask][nei] > dp[mask][node] + 1:
                    dp[nextMask][nei] = dp[mask][node] + 1
                    queue.append((nextMask, nei))

        result = float('inf')
        for i in range(n):
            result = min(result, dp[(1 << n) - 1][i])
        return result
```

Both approaches use BFS and dynamic programming with bitmask to efficiently find the shortest path to visit all nodes, ensuring optimality in solving the problem.

