# 542. [01 Matrix](https://leetcode.com/problems/01-matrix/)

## Approach: Breadth-First Search (BFS)

### Solution
```csharp
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the queue and distance matrix
using System.Collections.Generic;

public class Solution 
{
    public int[][] UpdateMatrix(int[][] mat) 
    {
        int rows = mat.Length;
        int cols = mat[0].Length;
        int[][] distances = new int[rows][];
        bool[][] visited = new bool[rows][];
        Queue<int[]> queue = new Queue<int[]>();

        // Step 1: Initialize the queue with all '0' cells and mark them as visited
        for (int i = 0; i < rows; i++) 
        {
            distances[i] = new int[cols];
            visited[i] = new bool[cols];
            for (int j = 0; j < cols; j++) 
            {
                if (mat[i][j] == 0) 
                {
                    queue.Enqueue(new int[] { i, j });
                    visited[i][j] = true;
                }
            }
        }

        // Step 2: Perform BFS to calculate distances
        int[][] directions = new int[][] 
        {
            new int[] { 0, 1 },
            new int[] { 1, 0 },
            new int[] { 0, -1 },
            new int[] { -1, 0 }
        };

        while (queue.Count > 0) 
        {
            int[] current = queue.Dequeue();
            foreach (int[] dir in directions) 
            {
                int newRow = current[0] + dir[0];
                int newCol = current[1] + dir[1];

                // Check bounds and if the cell is not visited
                if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && !visited[newRow][newCol]) 
                {
                    distances[newRow][newCol] = distances[current[0]][current[1]] + 1;
                    queue.Enqueue(new int[] { newRow, newCol });
                    visited[newRow][newCol] = true;
                }
            }
        }

        return distances;
    }
}
```

