# [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Approach 1: Simulation with Boundaries (Optimal)

### Solution
```java
// Time Complexity: O(m * n)
// Space Complexity: O(1) (excluding output list)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();

        if (matrix == null || matrix.length == 0) return result;

        int top = 0, bottom = matrix.length - 1;
        int left = 0, right = matrix[0].length - 1;

        while (top <= bottom && left <= right) {
            // Traverse from left to right
            for (int i = left; i <= right; i++) {
                result.add(matrix[top][i]);
            }
            top++;

            // Traverse from top to bottom
            for (int i = top; i <= bottom; i++) {
                result.add(matrix[i][right]);
            }
            right--;

            // Traverse from right to left
            if (top <= bottom) {
                for (int i = right; i >= left; i--) {
                    result.add(matrix[bottom][i]);
                }
                bottom--;
            }

            // Traverse from bottom to top
            if (left <= right) {
                for (int i = bottom; i >= top; i--) {
                    result.add(matrix[i][left]);
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
```java
// Time Complexity: O(m * n)
// Space Complexity: O(1) (excluding output list)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();

        if (matrix == null || matrix.length == 0) return result;

        int rows = matrix.length, cols = matrix[0].length;
        int startRow = 0, startCol = 0;

        while (startRow < rows && startCol < cols) {
            // Traverse the top row
            for (int i = startCol; i < cols; i++) {
                result.add(matrix[startRow][i]);
            }
            startRow++;

            // Traverse the right column
            for (int i = startRow; i < rows; i++) {
                result.add(matrix[i][cols - 1]);
            }
            cols--;

            // Traverse the bottom row
            if (startRow < rows) {
                for (int i = cols - 1; i >= startCol; i--) {
                    result.add(matrix[rows - 1][i]);
                }
                rows--;
            }

            // Traverse the left column
            if (startCol < cols) {
                for (int i = rows - 1; i >= startRow; i--) {
                    result.add(matrix[i][startCol]);
                }
                startCol++;
            }
        }

        return result;
    }
}
```