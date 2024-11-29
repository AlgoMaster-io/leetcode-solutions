# 1631. [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
csharp
```csharp
// Import necessary libraries
using System;
using System.Collections.Generic;

// Time Complexity: O(m * n * log(m * n)), where m is the number of rows and n is the number of columns.
// Space Complexity: O(m * n)
public class Solution {
    // Direction vectors for exploring adjacent cells (right, left, down, up)
    private static readonly int[][] DIRECTIONS = new int[][] {
        new int[] {0, 1}, new int[] {1, 0}, new int[] {0, -1}, new int[] {-1, 0}
    };

    public int MinimumEffortPath(int[][] heights) {
        int m = heights.Length;
        int n = heights[0].Length;

        // Min-heap priority queue to select the path with the minimum effort so far
        var minHeap = new SortedSet<(int, int, int)>(Comparer<(int, int, int)>.Create((a, b) =>
            a.Item3 == b.Item3 ? (a.Item1 == b.Item1 ? a.Item2.CompareTo(b.Item2) : a.Item1.CompareTo(b.Item1)) : a.Item3.CompareTo(b.Item3)));

        // Efforts matrix to track the minimum effort required to reach each cell
        int[][] efforts = new int[m][];
        for (int i = 0; i < m; i++) {
            efforts[i] = new int[n];
            Array.Fill(efforts[i], int.MaxValue);
        }
        efforts[0][0] = 0;

        // Start from the top-left corner of the grid
        minHeap.Add((0, 0, 0));

        while (minHeap.Count > 0) {
            // Extract the cell with the minimum effort
            var current = minHeap.Min;
            minHeap.Remove(current);
            int x = current.Item1;
            int y = current.Item2;
            int currentEffort = current.Item3;

            // If we've reached the bottom-right corner
            if (x == m - 1 && y == n - 1) {
                return currentEffort;
            }

            // Explore all adjacent cells
            foreach (var direction in DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];

                // Check bounds
                if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                    // Calculate the effort required to move to the adjacent cell
                    int effort = Math.Max(currentEffort, Math.Abs(heights[newX][newY] - heights[x][y]));

                    // If this path to the adjacent cell is better, update efforts and heap
                    if (effort < efforts[newX][newY]) {
                        efforts[newX][newY] = effort;
                        minHeap.Add((newX, newY, effort));
                    }
                }
            }
        }

        return 0; // Shouldn't reach here. Always possible to reach the end.
    }
}
```

## Approach 2: Binary Search + Depth First Search (DFS)

### Solution
csharp
```csharp
// Import necessary libraries
using System;
using System.Collections.Generic;

// Time Complexity: O(m * n * log(maxDifference))
// Space Complexity: O(m * n)
public class Solution {

    // Direction vectors for exploring adjacent cells (right, left, down, up)
    private static readonly int[][] DIRECTIONS = new int[][] {
        new int[] {0, 1}, new int[] {1, 0}, new int[] {0, -1}, new int[] {-1, 0}
    };

    public int MinimumEffortPath(int[][] heights) {
        int m = heights.Length;
        int n = heights[0].Length;
        int left = 0, right = 1_000_000, result = 1_000_000;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (CanReachEndWithEffort(heights, mid, m, n)) {
                result = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }

        return result;
    }

    // Helper function to determine if there is a path from (0, 0) to (m - 1, n - 1) under the given maxEffort
    private bool CanReachEndWithEffort(int[][] heights, int maxEffort, int m, int n) {
        var visited = new bool[m, n];
        var queue = new Queue<(int, int)>();
        queue.Enqueue((0, 0));
        visited[0,0] = true;

        while (queue.Count > 0) {
            var current = queue.Dequeue();
            int x = current.Item1;
            int y = current.Item2;

            if (x == m - 1 && y == n - 1) {
                return true;
            }

            foreach (var direction in DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];

                if (newX >= 0 && newX < m && newY >= 0 && newY < n && !visited[newX,newY]) {
                    int currentEffort = Math.Abs(heights[newX][newY] - heights[x][y]);

                    if (currentEffort <= maxEffort) {
                        visited[newX,newY] = true;
                        queue.Enqueue((newX, newY));
                    }
                }
            }
        }

        return false;
    }
}
```

