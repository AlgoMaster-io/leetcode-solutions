# [48. Rotate Image](https://leetcode.com/problems/rotate-image/)

## Approach 1: Transpose and Reverse Rows (Optimal)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;

        // Step 1: Transpose the matrix
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // Step 2: Reverse each row
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n / 2; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][n - j - 1];
                matrix[i][n - j - 1] = temp;
            }
        }
    }
}
```

## Approach 2: Rotating Layer by Layer (In-Place)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;

        // Rotate each layer
        for (int layer = 0; layer < n / 2; layer++) {
            int first = layer;
            int last = n - layer - 1;

            for (int i = first; i < last; i++) {
                int offset = i - first;

                // Save top element
                int top = matrix[first][i];

                // Move left to top
                matrix[first][i] = matrix[last - offset][first];

                // Move bottom to left
                matrix[last - offset][first] = matrix[last][last - offset];

                // Move right to bottom
                matrix[last][last - offset] = matrix[i][last];

                // Move top to right
                matrix[i][last] = top;
            }
        }
    }
}
```