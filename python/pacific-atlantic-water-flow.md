# [Leetcode 417: Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)

## Approaches:
- [Approach 1: Brute Force DFS](#approach-1-brute-force-dfs)
- [Approach 2: DFS with Optimization](#approach-2-dfs-with-optimization)
- [Approach 3: BFS for Both Oceans Simultaneously](#approach-3-bfs-for-both-oceans-simultaneously)

---

### Approach 1: Brute Force DFS

**Intuition**:  
A cell in the matrix can flow to both the Pacific and Atlantic oceans if starting from that cell we can perform a DFS to reach both ocean borders. In a brute-force approach, we perform a DFS starting at each cell and check if both end points can be reached by the flow path.

#### Steps:
1. For each cell, create two sets: `reachable_pacific` and `reachable_atlantic`.
2. From each cell, perform two distinct DFS:
   - One to check if flow reaches the top or left border (Pacific Ocean).
   - Another to check if flow reaches the bottom or right border (Atlantic Ocean).
3. If both searches return true, add this cell to the result.

#### Code:
```python
def pacificAtlantic(heights):
    if not heights or not heights[0]:
        return []

    def dfs(x, y, visited, ocean_identifier):
        if (x, y) in visited:
            return visited[(x, y)]
        visited[(x, y)] = False
        # Base case: if we reach the respective ocean border
        if ocean_identifier == "pacific" and (x == 0 or y == 0):
            return True
        if ocean_identifier == "atlantic" and (x == len(heights) - 1 or y == len(heights[0]) - 1):
            return True

        # Explore 4 directions
        for dx, dy in ((0, 1), (0, -1), (1, 0), (-1, 0)):
            new_x, new_y = x + dx, y + dy
            if 0 <= new_x < len(heights) and 0 <= new_y < len(heights[0]) and heights[new_x][new_y] <= heights[x][y]:
                if dfs(new_x, new_y, visited, ocean_identifier):
                    return True
        visited[(x, y)]=False
        return False

    result = []
    for i in range(len(heights)):
        for j in range(len(heights[0])):
            visited_pacific, visited_atlantic = {}, {}
            if dfs(i, j, visited_pacific, "pacific") and dfs(i, j, visited_atlantic, "atlantic"):
                result.append([i, j])
    return result
```

#### Complexity:
- **Time**: O((m*n) * (m + n)) for each DFS traversal.
- **Space**: O(m*n) for the visited sets.

---

### Approach 2: DFS with Optimization

**Intuition**:  
Instead of running DFS from each cell, reverse the process. Start from the ocean borders and perform a DFS to find cells that can reach each ocean. This reduces repeated work compared to the brute-force where each cell independently checks reachability.

#### Steps:
1. Create two sets: `pacific_reachable` and `atlantic_reachable` to track cells that can reach each ocean.
2. Perform DFS starting from all cells adjacent to the Pacific Ocean for `pacific_reachable`.
3. Perform a similar DFS starting from all cells adjacent to the Atlantic Ocean for `atlantic_reachable`.
4. The result will be the intersection of `pacific_reachable` and `atlantic_reachable`.

#### Code:
```python
def pacificAtlantic(heights):
    if not heights or not heights[0]:
        return []

    m, n = len(heights), len(heights[0])
    pacific_reachable = set()
    atlantic_reachable = set()

    def dfs(x, y, visited):
        visited.add((x, y))
        # Explore 4 potential neighbors
        for dx, dy in ((0, 1), (0, -1), (1, 0), (-1, 0)):
            new_x, new_y = x + dx, y + dy
            if (0 <= new_x < m and 0 <= new_y < n and
                (new_x, new_y) not in visited and
                heights[new_x][new_y] >= heights[x][y]):
                dfs(new_x, new_y, visited)

    # Perform DFS from each border where the oceans meet
    for i in range(m):
        dfs(i, 0, pacific_reachable)
        dfs(i, n-1, atlantic_reachable)
    for j in range(n):
        dfs(0, j, pacific_reachable)
        dfs(m-1, j, atlantic_reachable)

    return list(pacific_reachable & atlantic_reachable)
```

#### Complexity:
- **Time**: O(m*n) since each cell and its neighbors are processed once.
- **Space**: O(m*n) for the sets tracking visited cells.

---

### Approach 3: BFS for Both Oceans Simultaneously

**Intuition**:  
Similar to the DFS optimized solution, but using BFS from the ocean borders to explore all reachable cells. BFS is inherently iterative and might be more intuitive for some to avoid recursion limits.

#### Steps:
1. Initialize two queues for BFS, one for each ocean.
2. Perform BFS simultaneously from the Pacific and Atlantic borders.
3. Store reachable cells in sets and finally yield their intersection.

#### Code:
```python
from collections import deque

def pacificAtlantic(heights):
    if not heights or not heights[0]:
        return []

    m, n = len(heights), len(heights[0])
    pacific_reachable = set()
    atlantic_reachable = set()

    def bfs(queue, visited):
        while queue:
            x, y = queue.popleft()
            visited.add((x, y))
            for dx, dy in ((0, 1), (0, -1), (1, 0), (-1, 0)):
                new_x, new_y = x + dx, y + dy
                if (0 <= new_x < m and 0 <= new_y < n and
                    (new_x, new_y) not in visited and
                    heights[new_x][new_y] >= heights[x][y]):
                    visited.add((new_x, new_y))
                    queue.append((new_x, new_y))

    pacific_queue = deque([(i, 0) for i in range(m)] + [(0, j) for j in range(n)])
    atlantic_queue = deque([(i, n - 1) for i in range(m)] + [(m - 1, j) for j in range(n)])

    bfs(pacific_queue, pacific_reachable)
    bfs(atlantic_queue, atlantic_reachable)

    return list(pacific_reachable & atlantic_reachable)
```

#### Complexity:
- **Time**: O(m*n) as every cell is processed once.
- **Space**: O(m*n) for queues and sets.

Each approach builds upon the basic concept of reachability but optimizes the direction or method of search, from treating each cell as a start point to reversing the problem and using the ocean borders for initiation.

