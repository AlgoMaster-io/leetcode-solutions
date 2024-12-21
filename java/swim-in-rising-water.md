# [Leetcode 778: Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Binary Search with BFS/DFS](#approach-2-binary-search-with-bfsdfs)
- [Approach 3: Dijkstra's Algorithm with Min-Heap](#approach-3-dijkstras-algorithm-with-min-heap)

---

## Approach 1: Brute Force

### Intuition
The first approach is a brute force method, where we simulate the water level increasing over time and check if a path exists from the top-left to the bottom-right of the grid for each water level. This approach can be implemented using a BFS or DFS to explore the reachability of the target cell at each possible water level.

### Steps
1. Start iterating over water levels from 0 to the maximum possible water level in the grid.
2. For each water level, use a BFS or DFS to determine if there is a path from the top-left corner to the bottom-right corner where all visited cells are less than or equal to the current water level.
3. The first water level at which a path exists is the minimum time required to swim from the top left to the bottom right of the grid.

### Complexity
- **Time Complexity**: O(N^4), where N is the edge length of the grid. This is because we check each water level separately and perform a graph search on an N x N grid.
- **Space Complexity**: O(N^2) for the visited matrix and the queue used in BFS.

```java
public int swimInWater(int[][] grid) {
    int N = grid.length;
    int left = grid[0][0], right = N * N - 1;
    while (left < right) {
        int mid = (left + right) / 2;
        if (canSwim(mid, grid, N)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}

private boolean canSwim(int T, int[][] grid, int N) {
    boolean[][] visited = new boolean[N][N];
    return dfs(grid, visited, 0, 0, T);
}

private boolean dfs(int[][] grid, boolean[][] visited, int x, int y, int T) {
    int N = grid.length;
    if (x < 0 || y < 0 || x >= N || y >= N || visited[x][y] || grid[x][y] > T) {
        return false;
    }
    visited[x][y] = true;
    if (x == N - 1 && y == N - 1) return true;
    return dfs(grid, visited, x - 1, y, T) || dfs(grid, visited, x + 1, y, T)
            || dfs(grid, visited, x, y - 1, T) || dfs(grid, visited, x, y + 1, T);
}
```

---

## Approach 2: Binary Search with BFS/DFS

### Intuition
This approach improves upon the brute force method by applying binary search on the possible time value. For each middle value, we determine if the path can be formed using BFS/DFS as before. This significantly reduces the time complexity since we're not iterating through every possible water level but rather narrowing down the possibilities using binary search.

### Steps
1. Use binary search to find the minimum time required for the water level so a path exists.
2. For a middle point in the binary search range, use BFS/DFS to check for path existence.
3. Adjust the binary search range based on whether a path was successful or blocked.

### Complexity
- **Time Complexity**: O(N^2 * log(MaxVal)), where MaxVal is the maximum value (N * N - 1).
- **Space Complexity**: O(N^2) for visited structures.

```java
public int swimInWater(int[][] grid) {
    int N = grid.length;
    int left = grid[0][0], right = N * N - 1;
    while (left < right) {
        int mid = (left + right) / 2;
        if (canSwim(grid, mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}

private boolean canSwim(int[][] grid, int T) {
    int N = grid.length;
    boolean[][] visited = new boolean[N][N];
    return dfs(grid, visited, 0, 0, T);
}

private boolean dfs(int[][] grid, boolean[][] visited, int x, int y, int T) {
    int N = grid.length;
    if (x < 0 || y < 0 || x >= N || y >= N || visited[x][y] || grid[x][y] > T) {
        return false;
    }
    visited[x][y] = true;
    if (x == N - 1 && y == N - 1) return true;
    int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    for (int[] dir : directions) {
        if (dfs(grid, visited, x + dir[0], y + dir[1], T)) {
            return true;
        }
    }
    return false;
}
```

---

## Approach 3: Dijkstra's Algorithm with Min-Heap

### Intuition
The optimal approach for this problem is to use a priority queue (min-heap) to simulate Dijkstra's algorithm, which efficiently finds the minimum-path to the destination as the constraints evolve. Here, the priority queue helps in efficiently computing which cell to visit next based on the minimum possible water level encountered on the path.

### Steps
1. Use a priority queue to store cells along with their corresponding water level, starting from the top-left corner.
2. At each step, pop the least val cell from the queue, and mark it visited.
3. Push neighboring cells into the queue only if they are not visited.
4. Track the maximum water level on the current path, and if you reach the target cell (bottom-right), return that value as the result.

### Complexity
- **Time Complexity**: O(N^2 * log(N)), where N is the number of cells. The priority queue operations are logarithmic.
- **Space Complexity**: O(N^2) for visited structures and the priority queue storage.

```java
import java.util.PriorityQueue;
import java.util.Comparator;

public int swimInWater(int[][] grid) {
    int N = grid.length;
    boolean[][] visited = new boolean[N][N];
    PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
    pq.offer(new int[]{grid[0][0], 0, 0}); // {water_level, x, y}
    int[] directions = {-1, 0, 1, 0, -1};
    
    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int time = curr[0], x = curr[1], y = curr[2];
        
        if (visited[x][y]) continue;
        visited[x][y] = true;

        // If the bottom-right corner is reached
        if (x == N - 1 && y == N - 1) {
            return time;
        }

        // Look in the 4 possible directions
        for (int i = 0; i < 4; i++) {
            int nx = x + directions[i], ny = y + directions[i + 1];
            if (nx >= 0 && nx < N && ny >= 0 && ny < N && !visited[nx][ny]) {
                pq.offer(new int[]{Math.max(time, grid[nx][ny]), nx, ny});
            }
        }
    }
    return -1; // Just a fallback, logically unreachable
}
```

This approach efficiently calculates the minimum water level required to reach the target by always exploring the path with the lowest water level first, mimicking Dijkstra's algorithm on a grid with varying surface levels.

