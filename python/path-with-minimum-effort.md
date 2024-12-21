### [LeetCode 1631: Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

#### Approaches

- [Approach 1: Dijkstra's Algorithm with Min-Heap](#approach-1-dijkstras-algorithm-with-min-heap)
- [Approach 2: Binary Search with Union-Find](#approach-2-binary-search-with-union-find)

---

## Approach 1: Dijkstra's Algorithm with Min-Heap

### Intuition

The goal is to find a path from the top-left to the bottom-right of the grid such that the maximum difference in heights between consecutively chosen cells is minimized. Dijkstra's algorithm is well-suited for this type of "minimum maximum" problem as it could be used to expand the search starting from the minimum effort.

- Visualize the problem as a weighted graph where each cell in the grid is a node.
- The edge weights between nodes (cells) are the absolute differences in height.
- Use a priority queue (min-heap) to always expand the least recently visited, minimally weighted path.

### Detailed Steps

1. **Initialize a min-heap** with the starting cell (0,0) with an effort of 0.
2. Keep track of the minimum effort needed to reach each cell using a 2D list initialized to infinity.
3. Using the heap, expand the path to neighbors and calculate the possible effort required to reach those neighbors.
4. If reaching the neighbor through the current path has lower effort than previously recorded, update it and push the neighbor into the heap.
5. If the bottom-right cell is reached, return its effort value as the result.

### Code

```python
import heapq

def minimumEffortPath(heights):
    rows, cols = len(heights), len(heights[0])
    efforts = [[float('inf')] * cols for _ in range(rows)]
    efforts[0][0] = 0  # Starting at (0, 0) with zero effort
    
    # Min-heap; store tuples of (current_effort, x, y)
    heap = [(0, 0, 0)]
    
    # Directions for top, right, bottom, left movements
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    
    while heap:
        current_effort, x, y = heapq.heappop(heap)
        
        # If reached bottom-right corner
        if x == rows - 1 and y == cols - 1:
            return current_effort
        
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < rows and 0 <= ny < cols:
                # Calculate effort needed to move to neighbor (nx, ny)
                new_effort = max(current_effort, abs(heights[nx][ny] - heights[x][y]))
                
                # Only consider this path if it offers a better effort
                if new_effort < efforts[nx][ny]:
                    efforts[nx][ny] = new_effort
                    heapq.heappush(heap, (new_effort, nx, ny))
```

### Complexity

- **Time Complexity:** \(O(m \times n \log(m \times n))\) where \(m\) is the number of rows and \(n\) is the number of columns, as each cell is pushed into the heap at most once.
- **Space Complexity:** \(O(m \times n)\) for storing efforts and the heap.

---

## Approach 2: Binary Search with Union-Find

### Intuition

Another strategy is to change how we perceive the solution, opting for binary search combined with union-find:

- Use binary search to guess the answer — the minimum effort — and use union-find to check if that guessed effort allows a path from start to end.
- Start with assumptions for minimum and maximum efforts. Use the usual binary search structure.
- For each possible effort value (midpoint), try to connect all points with allowable effort using union-find.

### Detailed Steps

1. **Initialize union-find structures** to manage connectivity between cells.
2. Perform binary search on the range of possible efforts.
3. For each midpoint of the binary search, use union-find to check if there's a valid path connecting the top-left and bottom-right.
4. Adjust binary search bounds based on connectivity check until bounds converge.

### Code

```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size))
        self.rank = [1] * size
    
    def find(self, u):
        if self.parent[u] != u:
            self.parent[u] = self.find(self.parent[u])  # Path compression
        return self.parent[u]
    
    def union(self, u, v):
        rootU = self.find(u)
        rootV = self.find(v)
        if rootU != rootV:
            if self.rank[rootU] > self.rank[rootV]:
                self.parent[rootV] = rootU
            elif self.rank[rootU] < self.rank[rootV]:
                self.parent[rootU] = rootV
            else:
                self.parent[rootV] = rootU
                self.rank[rootU] += 1

def minimumEffortPath(heights):
    rows, cols = len(heights), len(heights[0])
    edges = []
    
    for r in range(rows):
        for c in range(cols):
            if r + 1 < rows:
                edges.append((abs(heights[r][c] - heights[r+1][c]), r * cols + c, (r+1) * cols + c))
            if c + 1 < cols:
                edges.append((abs(heights[r][c] - heights[r][c+1]), r * cols + c, r * cols + c + 1))
    
    edges.sort()  # sort edges by weight
    
    uf = UnionFind(rows * cols)
    
    left, right = 0, max(max(row) for row in heights)
    while left < right:
        mid = (left + right) // 2
        uf = UnionFind(rows * cols)
        
        for weight, u, v in edges:
            if weight <= mid:
                uf.union(u, v)
        
        if uf.find(0) == uf.find(rows * cols - 1):
            right = mid
        else:
            left = mid + 1
    
    return left
```

### Complexity

- **Time Complexity:** \(O(m \times n \times \log(\max(\text{heights})))\) because of binary search and up to \(O(m \times n)\) union operations.
- **Space Complexity:** \(O(m \times n)\) for the Union-Find structure.

