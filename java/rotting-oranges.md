# [Leetcode 994: Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

## Approaches
- [Approach 1: BFS (Breadth-First Search) Simulation](#approach-1-bfs-breadth-first-search-simulation)

## Approach 1: BFS (Breadth-First Search) Simulation

### Intuition
The problem is essentially about finding the shortest time it takes for all fresh oranges to become rotten, akin to a multi-source shortest path problem. The oranges that are initially rotten act as multiple starting points and rot adjacent fresh oranges in the four cardinal directions. We can utilize a breadth-first search (BFS) to simulate the process in levels, where each level represents one unit of time.

### Detailed Steps
1. **Initialize the Queue and Variables:**
   - Use a queue to implement BFS, starting with all initially rotten oranges.
   - Count the number of fresh oranges.
   - A time counter (`minutes`) to track the time it takes to rot all oranges.

2. **BFS Process:**
   - At each BFS level (each minute), process all the oranges in the queue (these are the oranges that rot in the current minute).
   - For each rotten orange, attempt to rot all adjacent fresh oranges.
   - If a fresh orange becomes rotten, decrement the fresh orange count and enqueue it.

3. **Check Results:**
   - If at the end of BFS, there are any remaining fresh oranges, it is impossible to rot all oranges, and we return `-1`.
   - Otherwise, return the time counter which now reflects the total minutes taken.

4. **Edge Cases:**
   - If there are no fresh oranges to begin with, return `0` since no time is required.
   - All oranges being rotten initially would also require `0` minutes.

### Time Complexity
- **Time Complexity:** \(O(m \times n)\), where \(m\) is the number of rows and \(n\) is the number of columns. Each cell in the grid is processed at most once.
- **Space Complexity:** \(O(m \times n)\) for the queue used in BFS.

### Java Code
```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int orangesRotting(int[][] grid) {
        // Coordinate shifts for adjacent cells (right, left, down, up)
        int[] dx = {0, 0, 1, -1};
        int[] dy = {1, -1, 0, 0};
        
        int rows = grid.length;
        int cols = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        int freshCount = 0;
        
        // Step 1: Initialize the queue with initially rotten oranges and count fresh oranges
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] == 2) {
                    queue.offer(new int[]{r, c});
                } else if (grid[r][c] == 1) {
                    freshCount++;
                }
            }
        }
        
        // Step 2: Apply BFS
        int minutes = 0;
        
        while (!queue.isEmpty() && freshCount > 0) {
            minutes++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] point = queue.poll();
                for (int d = 0; d < 4; d++) {
                    int newRow = point[0] + dx[d];
                    int newCol = point[1] + dy[d];
                    // Step 2.1: If this is a fresh orange, rot it
                    if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && grid[newRow][newCol] == 1) {
                        grid[newRow][newCol] = 2;
                        queue.offer(new int[]{newRow, newCol});
                        freshCount--;
                    }
                }
            }
        }
        
        // Step 3: If there are any fresh oranges left, return -1. Otherwise, return the minutes passed.
        return freshCount == 0 ? minutes : -1;
    }
}
```

This solution offers an efficient mechanism to handle the problem using a BFS-like propagation of the "infection" from initially rotten oranges, ensuring that the problem is tackled in optimal time and space complexity.

