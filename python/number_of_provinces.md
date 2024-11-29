# 547. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Approach 1: Depth-First Search (DFS)

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        def dfs(i):
            visited[i] = True
            for j in range(len(isConnected)):
                if isConnected[i][j] == 1 and not visited[j]:
                    dfs(j)

        n = len(isConnected)
        visited = [False] * n
        provinces = 0
       
        for i in range(n):
            if not visited[i]:
                dfs(i)
                provinces += 1

        return provinces
```

## Approach 2: Breadth-First Search (BFS)

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
from collections import deque

class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        def bfs(i):
            queue = deque([i])
            visited[i] = True
            while queue:
                city = queue.popleft()
                for j in range(len(isConnected)):
                    if isConnected[city][j] == 1 and not visited[j]:
                        queue.append(j)
                        visited[j] = True
        
        n = len(isConnected)
        visited = [False] * n
        provinces = 0
        
        for i in range(n):
            if not visited[i]:
                bfs(i)
                provinces += 1

        return provinces
```

## Approach 3: Union-Find

### Solution
python
```python
# Time Complexity: O(n^2 * α(n))
# Space Complexity: O(n)
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.count = n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # Path compression
        return self.parent[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            self.parent[rootX] = rootY  # Union by root
            self.count -= 1

    def getCount(self):
        return self.count

class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        n = len(isConnected)
        uf = UnionFind(n)

        for i in range(n):
            for j in range(n):
                if isConnected[i][j] == 1:
                    uf.union(i, j)

        return uf.getCount()
```

