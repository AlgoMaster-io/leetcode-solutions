[Leetcode Problem 827: Making A Large Island](https://leetcode.com/problems/making-a-large-island/)

### Solutions:

- [Approach 1: Brute Force – Simulate Conversion](#approach-1-brute-force-–-simulate-conversion)
- [Approach 2: Optimized Union-Find with Pre-Processing](#approach-2-optimized-union-find-with-pre-processing)

---

### Approach 1: Brute Force – Simulate Conversion

#### Intuition:
In this approach, we systematically simulate converting every zero to a one, and for each conversion, we compute the size of the connected island it would create. This involves depth-first search (DFS) to count the size of the connected component (island) once the conversion is made. Naturally, this approach can become inefficient for large grids owing to the need to repeatedly recompute island sizes from scratch.

#### Steps:
1. Traverse each cell in the grid.
2. For every zero found, temporarily change it to one.
3. Use DFS from this cell to calculate the island's size.
4. Restore the grid to its original state.
5. Track the maximum island size observed during these simulations.

Here is the implementation in Python:

```python
def largestIsland(grid):
    def dfs(r, c):
        if 0 <= r < len(grid) and 0 <= c < len(grid[0]) and grid[r][c] == 1:
            grid[r][c] = -1  # Mark the land visited
            return 1 + dfs(r + 1, c) + dfs(r - 1, c) + dfs(r, c + 1) + dfs(r, c - 1)
        return 0

    max_island = 0
    has_zero = False

    for r in range(len(grid)):
        for c in range(len(grid[0])):
            if grid[r][c] == 0:
                has_zero = True
                grid[r][c] = 1
                max_island = max(max_island, dfs(r, c))
                grid[r][c] = 0

    return max_island if has_zero else len(grid) * len(grid[0])
```

#### Time Complexity:
- O(n^4): For each zero, we explore the entire grid in the worst case.

#### Space Complexity:
- O(n^2): Due to the recursive stack used in DFS.

---

### Approach 2: Optimized Union-Find with Pre-Processing

#### Intuition:
Instead of recalculating island sizes from scratch, we precompute the size of each connected component using a two-pass approach. The first pass assigns unique identifiers (IDs) to islands while calculating their sizes. The second pass explores where converting a zero can unite different islands to ascertain the size of the newly formed island.

#### Steps:
1. Identify each island in the grid with a unique ID and calculate its area using DFS.
2. Store each island's area in a dictionary keyed by their ID.
3. For each zero in the grid, find the set of unique neighboring island IDs and sum up their areas (including the converted zero).
4. Track the maximum sum observed.

Here is the implementation in Python:

```python
def largestIsland(grid):
    n = len(grid)

    def dfs(r, c, island_id):
        if 0 <= r < n and 0 <= c < n and grid[r][c] == 1:
            grid[r][c] = island_id
            return 1 + dfs(r + 1, c, island_id) + dfs(r - 1, c, island_id) + dfs(r, c + 1, island_id) + dfs(r, c - 1, island_id)
        return 0

    island_area = {}
    island_id = 2

    for r in range(n):
        for c in range(n):
            if grid[r][c] == 1:
                island_area[island_id] = dfs(r, c, island_id)
                island_id += 1

    max_island = max(island_area.values() or [0])

    dirs = [(0, 1), (1, 0), (0, -1), (-1, 0)]

    for r in range(n):
        for c in range(n):
            if grid[r][c] == 0:
                seen = set()
                for dr, dc in dirs:
                    nr, nc = r + dr, c + dc
                    if 0 <= nr < n and 0 <= nc < n and grid[nr][nc] > 1:
                        seen.add(grid[nr][nc])
                possible_area = 1  # this zero becomes a one
                for i in seen:
                    possible_area += island_area[i]
                max_island = max(max_island, possible_area)

    return max_island
```

#### Time Complexity:
- O(n^2): Linear traversal and DFS exploration.

#### Space Complexity:
- O(n^2): For the grid and island mappings.

