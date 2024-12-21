# [Leetcode 1293: Shortest Path in a Grid with Obstacles Elimination](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

## Approaches:
- [Approach 1: Breadth-First Search (BFS) with State Tracking](#approach-1-breadth-first-search-bfs-with-state-tracking)

---

## Approach 1: Breadth-First Search (BFS) with State Tracking

### Intuition
Given a grid, you need to find the shortest path from the top-left corner to the bottom-right corner while allowing a specified number of obstacles to be removed. A natural choice for problems involving shortest paths is the BFS algorithm, which will explore all nodes at the present "depth" before moving on to nodes at the next depth level.

We need to consider an additional state here, which is the number of obstacles eliminated so far at each node. This state is crucial because we can reach the same cell with different numbers of eliminations, affecting the rest of the path.

### Key Steps
1. **State Representation**: Each point `(x, y)` will be represented as `(x, y, eliminated)` in our BFS queue, where `eliminated` is the number of obstacles removed to reach this cell.
2. **Queue Initialization**: Start with the initial position `(0, 0, 0)` in the queue. The third dimension `0` represents that no obstacles have been eliminated so far.
3. **State Tracking**: Use a 3D boolean array `visited[x][y][eliminated]` to track if a particular state has been explored.
4. **Explore Neighbors**: For each cell, look at potential moves (up, down, left, right). If moving to a new cell with an obstacle, increment the `eliminated` count.
5. **Termination and Optimization**: If at any point the bottom-right cell is reached, return the number of steps taken. If no valid path is found, return `-1`.

### Time and Space Complexity
- **Time Complexity**: \(O(m \times n \times k)\), where \(m\) is the number of rows, \(n\) the number of columns, and \(k\) is the maximum number of obstacles we can eliminate. Each cell can be visited multiple times based on the number of obstacles that can be eliminated.
- **Space Complexity**: \(O(m \times n \times k)\) due to the additional state tracking.

```typescript
function shortestPath(grid: number[][], k: number): number {
    const m = grid.length;
    const n = grid[0].length;
    
    // Direction vectors for up, down, left, right movements
    const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];

    // Queue for BFS: elements are arrays of type [x, y, eliminated, steps]
    const queue: [number, number, number, number][] = [[0, 0, 0, 0]];

    // Visited state: 3D array to keep track of visited states [x][y][eliminated]
    const visited = Array.from({ length: m }, 
                    () => Array.from({ length: n }, 
                    () => Array(k + 1).fill(false)));

    // Start state visited
    visited[0][0][0] = true;

    while (queue.length > 0) {
        const [x, y, eliminations, steps] = queue.shift()!;

        // If we reach the bottom-right corner, return the number of steps
        if (x === m - 1 && y === n - 1) {
            return steps;
        }

        // Explore neighbors
        for (const [dx, dy] of directions) {
            const nx = x + dx;
            const ny = y + dy;
            const newEliminations = eliminations + (grid[nx]?.[ny] || 0);

            // Continue if out of bounds or exceeds the elimination limit
            if (nx < 0 || nx >= m || ny < 0 || ny >= n || newEliminations > k) {
                continue;
            }

            // If this state has not been visited yet, add to queue
            if (!visited[nx][ny][newEliminations]) {
                visited[nx][ny][newEliminations] = true;
                queue.push([nx, ny, newEliminations, steps + 1]);
            }
        }
    }

    // If no path is found, return -1
    return -1;
}
```

In this problem, BFS is a perfect choice due to its ability to explore the shortest path effectively, especially when combined with a state-tracking mechanism to handle the constraints of obstacle elimination.

