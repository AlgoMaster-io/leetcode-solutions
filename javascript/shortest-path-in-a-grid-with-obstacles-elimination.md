# [Leetcode 1293: Shortest Path in a Grid with Obstacles Elimination](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

## Approaches:
1. [Breadth-First Search (BFS) with Early Stopping](#approach-1-breadth-first-search-bfs-with-early-stopping)

---

## Approach 1: Breadth-First Search (BFS) with Early Stopping

**Intuition:**

We use BFS because it explores all possibilities level by level and guarantees the shortest path in an unweighted grid like this. This problem, however, adds the complication of obstacles, which we can "eliminate." Hence, our state in the BFS needs to track:
- The current position `(x, y)` in the grid.
- The number of remaining obstacles we can eliminate `k`.

We can optimize the BFS by stopping early when we reach the goal `(m-1, n-1)` and keeping track of the minimum steps to the target position with remaining obstacle eliminations.

**Algorithm:**

1. Initialize a queue for BFS and a 3D array `visited` where `visited[x][y][k]` tracks if the position `(x, y)` with `k` remaining eliminations has been visited.
2. Enqueue the starting position `(0, 0)` with initial eliminations `k` and step count `0`.
3. Perform BFS:
   - Dequeue the current position and determine if it's the target.
   - For each of the 4 possible movements (up, down, left, right):
     - Calculate new positions and check boundaries.
     - Use an obstacle eliminations count to determine if the position can be accessed.
     - Check if the position can be visited with the remaining eliminations.
     - Update the queue with new positions, steps, and remaining eliminations.
4. If we reach the target, return the step count; otherwise, return `-1`.

**Code Implementation:**

```javascript
function shortestPath(grid, k) {
    const m = grid.length;
    const n = grid[0].length;

    // Directions for moving in the grid (right, down, left, up)
    const directions = [
        [0, 1], [1, 0], [0, -1], [-1, 0]
    ];

    // Initializing the queue for BFS and the visited map
    let queue = [[0, 0, k, 0]]; // [row, col, remaining_k, steps]
    const visited = Array.from({ length: m }, () =>
        Array.from({ length: n }, () => Array(k + 1).fill(false))
    );
    visited[0][0][k] = true;

    // Start BFS
    while (queue.length > 0) {
        const [x, y, remainingK, steps] = queue.shift();

        // If we have reached the bottom-right corner
        if (x === m - 1 && y === n - 1) {
            return steps;
        }

        // Explore the 4 directions
        for (const [dx, dy] of directions) {
            const newX = x + dx;
            const newY = y + dy;

            // Check boundaries
            if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                const newK = remainingK - grid[newX][newY];

                // If we have eliminations remaining and haven't visited this state
                if (newK >= 0 && !visited[newX][newY][newK]) {
                    visited[newX][newY][newK] = true;
                    queue.push([newX, newY, newK, steps + 1]);
                }
            }
        }
    }

    // If we exhaust the queue without reaching the target
    return -1;
}
```

**Time Complexity:** O(m * n * k)
- Each cell could potentially be visited with different remaining eliminations up to `k`.

**Space Complexity:** O(m * n * k)
- We store the `visited` states in a 3D array to track `(x, y, k)` visited states.

