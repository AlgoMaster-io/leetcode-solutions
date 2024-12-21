# [Leetcode 1631: Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Approaches
- [Approach 1: Depth First Search with Binary Search](#approach-1-depth-first-search-with-binary-search)
- [Approach 2: Dijkstra's Algorithm](#approach-2-dijkstras-algorithm)

## Approach 1: Depth First Search with Binary Search

### Intuition:
In this approach, the problem is transformed into finding the minimum effort we need to traverse the path by making use of binary search. Essentially, we perform binary search over possible efforts, and for every mid value in binary search, we use a Depth First Search (DFS) to verify if it's feasible to reach the destination with that maximum effort.

### Explanation:
1. **Binary Search**: Perform binary search on the range of possible efforts, from 0 to the maximum absolute difference in heights between any two points.
2. **DFS**: For each value of effort checked in the binary search, use DFS to check if you can reach from the top-left corner to the bottom-right corner without exceeding the given effort.

```csharp
public class Solution {
    private int RowCount;
    private int ColCount;

    public int MinimumEffortPath(int[][] heights) {
        RowCount = heights.Length;
        ColCount = heights[0].Length;
        
        int left = 0, right = 1_000_000;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (CanReachDestination(heights, mid)) {
                right = mid;  // Try smaller effort since mid is possible
            } else {
                left = mid + 1;  // Increase effort as mid is not possible
            }
        }
        
        return left;
    }

    private bool CanReachDestination(int[][] heights, int maxEffort) {
        // Reinitialize visited nodes
        var visited = new bool[RowCount, ColCount];
        return Dfs(heights, visited, 0, 0, maxEffort);
    }

    private bool Dfs(int[][] heights, bool[,] visited, int x, int y, int maxEffort) {
        if (x == RowCount - 1 && y == ColCount - 1) return true;
        
        // Mark the current cell as visited
        visited[x, y] = true;
        
        int[][] directions = { new int[] { 0, 1 }, new int[] { 1, 0 }, new int[] { 0, -1 }, new int[] { -1, 0 } };
        
        foreach (var dir in directions) {
            int newX = x + dir[0];
            int newY = y + dir[1];
            
            if (newX >= 0 && newX < RowCount && newY >= 0 && newY < ColCount && !visited[newX, newY]) {
                int diff = Math.Abs(heights[newX][newY] - heights[x][y]);
                
                if (diff <= maxEffort) {
                    if (Dfs(heights, visited, newX, newY, maxEffort)) {
                        return true;
                    }
                }
            }
        }
        
        return false;
    }
}
```

### Time Complexity:
- **Binary Search**: O(log(max_heights_difference))
- **DFS**: O(R * C) per Effort level
- Overall: O(R * C * log(max_heights_difference))

### Space Complexity:
- O(R * C) for visited array.

## Approach 2: Dijkstra's Algorithm

### Intuition:
This problem can also be seen as a graph problem where each pixel in the grid is a node and the edges are defined by the height differences between the nodes. Dijkstra's algorithm can be applied to find the shortest path where the path cost is defined by the maximum height difference along the path.

### Explanation:
1. Utilize a min-heap to explore nodes by the least maximum effort path possible.
2. Update the effort for each adjacent node if a lesser effort path is found.

```csharp
public class Solution {
    private static readonly int[][] Directions = new int[][] {
        new int[] { 0, 1 }, new int[] { 1, 0 }, new int[] { 0, -1 }, new int[] { -1, 0 }
    };

    public int MinimumEffortPath(int[][] heights) {
        int rowCount = heights.Length;
        int colCount = heights[0].Length;
        
        var efforts = new int[rowCount, colCount];
        for (int i = 0; i < rowCount; i++) {
            for (int j = 0; j < colCount; j++) {
                efforts[i, j] = int.MaxValue;
            }
        }
        efforts[0, 0] = 0;
        
        var pq = new PriorityQueue<(int, int, int), int>();
        pq.Enqueue((0, 0, 0), 0);  // (x, y, effort)

        while (pq.Count > 0) {
            var (x, y, currentEffort) = pq.Dequeue();

            // If arrived at the destination
            if (x == rowCount - 1 && y == colCount - 1) {
                return currentEffort;
            }

            foreach (var d in Directions) {
                int newX = x + d[0];
                int newY = y + d[1];

                if (newX >= 0 && newX < rowCount && newY >= 0 && newY < colCount) {
                    int effort = Math.Max(currentEffort, Math.Abs(heights[newX][newY] - heights[x][y]));

                    if (effort < efforts[newX, newY]) {
                        efforts[newX, newY] = effort;
                        pq.Enqueue((newX, newY, effort), effort);
                    }
                }
            }
        }

        return 0;  // should never reach here
    }
}
```

### Time Complexity:
- O((R * C) * log(R * C)), where R is the number of rows and C is the number of columns.

### Space Complexity:
- O(R * C) for the effort matrix and priority queue.

