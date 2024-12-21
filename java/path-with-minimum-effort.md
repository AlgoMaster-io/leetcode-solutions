# [Leetcode 1631: Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Approaches

1. [DFS with Backtracking](#dfs-with-backtracking)
2. [Binary Search with BFS](#binary-search-with-bfs)
3. [Dijkstra's Algorithm with Min-Heap](#dijkstras-algorithm-with-min-heap)

---

### DFS with Backtracking

The simplest solution is to use Depth-First Search (DFS) to explore all possible paths. For each path, we calculate the maximum effort and keep track of the minimum effort path found. The algorithm explores all paths and can thus guarantee that it finds the minimum effort path.

#### Intuition:

- Use DFS to explore paths from the top-left to the bottom-right.
- Record the efforts in each path and maintain the minimum path effort found.
- Use backtracking to ensure all possible paths are explored.

#### Java Code:

```java
public class Solution {
    private int minEffort;
    
    public int minimumEffortPath(int[][] heights) {
        minEffort = Integer.MAX_VALUE;
        boolean[][] visited = new boolean[heights.length][heights[0].length];
        dfs(heights, visited, 0, 0, 0, -1);
        return minEffort;
    }
    
    private void dfs(int[][] heights, boolean[][] visited, int x, int y, int currentEffort, int prevHeight) {
        // Base case: reach bottom-right corner
        if (x == heights.length - 1 && y == heights[0].length - 1) {
            minEffort = Math.min(minEffort, currentEffort);
            return;
        }

        // Mark the current cell as visited
        visited[x][y] = true;

        // Possible directions
        int[] dx = {0, 0, 1, -1};
        int[] dy = {1, -1, 0, 0};

        // Explore all 4 directions
        for (int i = 0; i < 4; i++) {
            int newX = x + dx[i];
            int newY = y + dy[i];

            // Check boundaries and if the cell is not visited
            if (newX >= 0 && newY >= 0 && newX < heights.length && newY < heights[0].length && !visited[newX][newY]) {
                int effort = Math.abs(heights[newX][newY] - heights[x][y]);
                // Explore the path by a recursive DFS call
                dfs(heights, visited, newX, newY, Math.max(currentEffort, effort), heights[x][y]);
            }
        }

        // Backtrack
        visited[x][y] = false;
    }
}
```

#### Time Complexity:
- **O((M*N)!)**: In the worst case, each cell tries all paths.

#### Space Complexity:
- **O(M * N)**: Space for visited matrix.

---

### Binary Search with BFS

We can optimize the approach by using binary search along with a BFS for path verification. The key idea is that the minimum effort path must lie within a specific range, and this range can be narrowed down using binary search.

#### Intuition:

- Perform a binary search on the range of effort to find the minimum possible effort.
- For each mid effort, use BFS to check if there exists a valid path from start to end.

#### Java Code:

```java
public class Solution {
    private final int[] directionX = {1, 0, -1, 0};
    private final int[] directionY = {0, 1, 0, -1};

    public int minimumEffortPath(int[][] heights) {
        int left = 0, right = 1000000;

        while (left < right) {
            int mid = (left + right) / 2;
            if (canReachEnd(heights, mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        return left;
    }

    private boolean canReachEnd(int[][] heights, int mid) {
        int m = heights.length, n = heights[0].length;
        boolean[][] visited = new boolean[m][n];
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, 0});
        visited[0][0] = true;

        while (!queue.isEmpty()) {
            int[] point = queue.poll();
            int x = point[0], y = point[1];

            if (x == m - 1 && y == n - 1) {
                return true;
            }

            for (int i = 0; i < 4; i++) {
                int newX = x + directionX[i];
                int newY = y + directionY[i];

                if (newX >= 0 && newX < m && newY >= 0 && newY < n && !visited[newX][newY]) {
                    int currentEffort = Math.abs(heights[newX][newY] - heights[x][y]);
                    if (currentEffort <= mid) {
                        queue.add(new int[]{newX, newY});
                        visited[newX][newY] = true;
                    }
                }
            }
        }

        return false;
    }
}
```

#### Time Complexity:
- **O(log(max_height) * M * N)**: Binary search combined with BFS check over the matrix.

#### Space Complexity:
- **O(M * N)**: Space for visited matrix and queue.

---

### Dijkstra's Algorithm with Min-Heap

The most efficient approach is to use a modified version of Dijkstra's algorithm to keep track of the minimum effort distance via a priority queue (min-heap).

#### Intuition:

- Treat each cell as a node and update efforts using min-heap.
- Always expand the node with the smallest effort difference until reaching the bottom-right cell.

#### Java Code:

```java
import java.util.PriorityQueue;
import java.util.Comparator;

public class Solution {
    private final int[] directionX = {1, 0, -1, 0};
    private final int[] directionY = {0, 1, 0, -1};

    public int minimumEffortPath(int[][] heights) {
        int m = heights.length, n = heights[0].length;
        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        pq.offer(new int[]{0, 0, 0}); // effort, row, col
        int[][] efforts = new int[m][n];
        for (int[] row : efforts) {
            Arrays.fill(row, Integer.MAX_VALUE);
        }

        while (!pq.isEmpty()) {
            int[] current = pq.poll();
            int effort = current[0], x = current[1], y = current[2];

            // If reached bottom-right cell, return the effort.
            if (x == m - 1 && y == n - 1) {
                return effort;
            }

            for (int i = 0; i < 4; i++) {
                int newX = x + directionX[i];
                int newY = y + directionY[i];
                if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                    int currentEffort = Math.max(effort, Math.abs(heights[newX][newY] - heights[x][y]));
                    if (currentEffort < efforts[newX][newY]) {
                        efforts[newX][newY] = currentEffort;
                        pq.offer(new int[]{currentEffort, newX, newY});
                    }
                }
            }
        }

        return 0; // unreachable
    }
}
```

#### Time Complexity:
- **O(M * N * log(M * N))**: Each cell is processed via the priority queue which maintains its order logarithmically.

#### Space Complexity:
- **O(M * N)**: To keep track of efforts to reach each cell.

