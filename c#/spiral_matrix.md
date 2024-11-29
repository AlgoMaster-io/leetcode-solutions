# 54. [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Approach 1: Simulation with Boundaries (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(m * n)
// Space Complexity: O(1) (excluding output list)
using System.Collections.Generic;

public class Solution {
    public IList<int> SpiralOrder(int[][] matrix) {
        var result = new List<int>();

        if (matrix == null || matrix.Length == 0) return result;

        int top = 0, bottom = matrix.Length - 1;
        int left = 0, right = matrix[0].Length - 1;

        while (top <= bottom && left <= right) {
            // Traverse from left to right
            for (int i = left; i <= right; i++) {
                result.Add(matrix[top][i]);
            }
            top++;

            // Traverse from top to bottom
            for (int i = top; i <= bottom; i++) {
                result.Add(matrix[i][right]);
            }
            right--;

            // Traverse from right to left
            if (top <= bottom) {
                for (int i = right; i >= left; i--) {
                    result.Add(matrix[bottom][i]);
                }
                bottom--;
            }

            // Traverse from bottom to top
            if (left <= right) {
                for (int i = bottom; i >= top; i--) {
                    result.Add(matrix[i][left]);
                }
                left++;
            }
        }

        return result;
    }
}
```

## Approach 2: Simulated Layer Traversal (Alternative)

### Solution
csharp
```csharp
// Time Complexity: O(m * n)
// Space Complexity: O(1) (excluding output list)
using System.Collections.Generic;

public class Solution {
    public IList<int> SpiralOrder(int[][] matrix) {
        var result = new List<int>();

        if (matrix == null || matrix.Length == 0) return result;

        int rows = matrix.Length, cols = matrix[0].Length;
        int startRow = 0, startCol = 0;

        while (startRow < rows && startCol < cols) {
            // Traverse the top row
            for (int i = startCol; i < cols; i++) {
                result.Add(matrix[startRow][i]);
            }
            startRow++;

            // Traverse the right column
            for (int i = startRow; i < rows; i++) {
                result.Add(matrix[i][cols - 1]);
            }
            cols--;

            // Traverse the bottom row
            if (startRow < rows) {
                for (int i = cols - 1; i >= startCol; i--) {
                    result.Add(matrix[rows - 1][i]);
                }
                rows--;
            }

            // Traverse the left column
            if (startCol < cols) {
                for (int i = rows - 1; i >= startRow; i--) {
                    result.Add(matrix[i][startCol]);
                }
                startCol++;
            }
        }

        return result;
    }
}
```

