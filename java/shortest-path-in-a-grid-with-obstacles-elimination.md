# [1293. Shortest Path in a Grid with Obstacles Elimination](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

## Approaches
1. [Breadth-First Search (BFS) with State Tracking](#approach-1-bfs-with-state-tracking)
2. [Optimized BFS using A* Heuristic](#approach-2-optimized-bfs-using-a-astar-heuristic)

---

## Approach 1: BFS with State Tracking

### Intuition
The problem requires finding the shortest path in a grid while overcoming obstacles with a limited number of eliminations allowed for obstacles. A natural approach to explore all possible paths in an unweighted grid and account for the obstacle eliminations is to use Breadth-First Search (BFS). 

In this solution:
- We treat the grid as a graph where each cell is a node.
- Edges exist between each node (cell) to its neighbor nodes (adjacent cells).
- We incorporate additional state variables in our search to account for the number of eliminations performed so far and only proceed if doing so does not exceed the maximum allowable eliminations.
- We use a 3D boolean array to track visited states, ensuring that we do not process the same state multiple times unnecessarily.

### Code
```java
public int shortestPath(int[][] grid, int k) {
    int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    int rows = grid.length;
    int cols = grid[0].length;
    
    // Queue to store the tuple (x, y, currentObstacleEliminationCount)
    Queue<int[]> queue = new LinkedList<>();
    // A visited array tracking if a specific state has been visited: (r, c, eliminations)
    boolean[][][] visited = new boolean[rows][cols][k + 1];
    
    // Start BFS
    queue.offer(new int[]{0, 0, 0});
    visited[0][0][0] = true;
    int steps = 0;

    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int[] current = queue.poll();
            int x = current[0], y = current[1], eliminations = current[2];

            // Check if we've reached the bottom-right corner
            if (x == rows - 1 && y == cols - 1) {
                return steps;
            }

            // Explore neighbors
            for (int[] direction : directions) {
                int newX = x + direction[0];
                int newY = y + direction[1];

                // Continue if new position is out of bounds
                if (newX < 0 || newX >= rows || newY < 0 || newY >= cols) {
                    continue;
                }

                int newEliminations = eliminations + grid[newX][newY]; 

                // Continue if this state is not allowed
                if (newEliminations > k || visited[newX][newY][newEliminations]) {
                    continue;
                }

                // Mark this state as visited and add to the queue
                visited[newX][newY][newEliminations] = true;
                queue.offer(new int[]{newX, newY, newEliminations});
            }
        }
        steps++;
    }
    return -1;
}
```

### Time and Space Complexity
- **Time Complexity**: \(O(N \times M \times K)\), where \(N\) is the number of rows, \(M\) is the number of columns, and \(K\) is the limit on obstacle eliminations.
- **Space Complexity**: \(O(N \times M \times K)\) due to the space needed to store visited states.

---

## Approach 2: Optimized BFS using A* Heuristic

### Intuition
This approach enhances the BFS by adding an A* heuristic to prioritize the exploration of paths that seem promising, based on a heuristic function that estimates the cost to reach the destination. The heuristic used can be the Manhattan distance to the bottom-right corner. By incorporating the A* heuristic, we guide the BFS to expand nodes that are likely part of the shortest path, potentially reducing the number of states we explore.

### Code
```java
public int shortestPathOptimized(int[][] grid, int k) {
    int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    int rows = grid.length;
    int cols = grid[0].length;

    PriorityQueue<int[]> pq = new PriorityQueue<>(
        Comparator.comparingInt(a -> a[0] + heuristic(a[1], a[2], rows, cols))
    );

    boolean[][][] visited = new boolean[rows][cols][k + 1];

    pq.offer(new int[]{0, 0, 0, 0});
    visited[0][0][0] = true;

    while (!pq.isEmpty()) {
        int[] current = pq.poll();
        int curSteps = current[0], x = current[1], y = current[2], eliminations = current[3];

        if (x == rows - 1 && y == cols - 1) {
            return curSteps;
        }

        for (int[] direction : directions) {
            int newX = x + direction[0];
            int newY = y + direction[1];

            // Skip out of bounds
            if (newX < 0 || newX >= rows || newY < 0 || newY >= cols) {
                continue;
            }

            int newEliminations = eliminations + grid[newX][newY];

            if (newEliminations > k || visited[newX][newY][newEliminations]) {
                continue;
            }

            visited[newX][newY][newEliminations] = true;
            pq.offer(new int[]{curSteps + 1, newX, newY, newEliminations});
        }
    }
    return -1;
}

private int heuristic(int x, int y, int rows, int cols) {
    return (rows - 1 - x) + (cols - 1 - y);
}
```

### Time and Space Complexity
- **Time Complexity**: Typically better than plain BFS, potentially \(O(N \times M \times K)\) in worst-case but often reduced by choosing promising nodes earlier.
- **Space Complexity**: \(O(N \times M \times K)\) similar to BFS due to state storage.

Both approaches provide solutions to finding the shortest path in a grid considering obstacle eliminations, with the second approach using an A* heuristic to accelerate the search by exploring promising paths first.

