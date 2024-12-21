# [LeetCode 1277: Count Square Submatrices with All Ones](https://leetcode.com/problems/count-square-submatrices-with-all-ones/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)

## Brute Force Approach

### Intuition
The brute force approach is straightforward: we traverse each cell in the matrix and for each '1', we try to expand it to see how large a square of ones we can form that starts at that cell. We continue this until the largest possible square from that position is found. Accumulate the counts of all possible squares.

### Steps:
1. Traverse through the matrix cell by cell.
2. Whenever a '1' is encountered, try to expand into a square as much as possible by checking each layer outside the current square.
3. For each expansion, check if all the elements in the expanded square are '1'.
4. Maintain a counter to count all the valid squares formed.

### Code

```csharp
public class Solution {
    public int CountSquares(int[][] matrix) {
        int count = 0;
        int rows = matrix.Length;
        int cols = matrix[0].Length;

        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                // If the current cell is 1, check the possible squares
                if (matrix[r][c] == 1) {
                    int maxSize = Math.Min(rows - r, cols - c);
                    for (int size = 1; size <= maxSize; size++) {
                        bool isValidSquare = true;
                        // Check the new row
                        for (int k = 0; k < size; k++) {
                            if (matrix[r + size - 1][c + k] == 0) {
                                isValidSquare = false;
                                break;
                            }
                        }
                        // Check the new column
                        for (int k = 0; k < size; k++) {
                            if (matrix[r + k][c + size - 1] == 0) {
                                isValidSquare = false;
                                break;
                            }
                        }
                        
                        if (isValidSquare) {
                            count++;
                        } else {
                            break; // No need to check further if the current size fails
                        }
                    }
                }
            }
        }
        return count;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O((m * n) * min(m, n)), where `m` is the number of rows and `n` is the number of columns. This arises because for each cell, we potentially explore squares that can have a side length equal to `min(m, n)`.
- **Space Complexity**: O(1), as we're only using a fixed amount of space regardless of input size.

---

## Dynamic Programming Approach

### Intuition
Instead of recalculating valid square sizes many times like in the brute force method, we can use dynamic programming to store the size of the largest square submatrix whose bottom-right corner is a given cell. For a cell `(r, c)`, this is 1 plus the minimum of the squares to its left, above, and diagonally above-left, if it itself is a '1'. This leads to an efficient evaluation of possible squares as the information already calculated can be reused.

### Steps:
1. Create a DP matrix of the same dimensions as the input matrix.
2. Traverse each cell of the input matrix:
   - If the current cell `(r, c)` is a `1` and it is not on the first row or column, update `dp[r][c]` as
     `1 + Min(dp[r-1][c], dp[r][c-1], dp[r-1][c-1])`.
   - If the cell is on the first row or column, then `dp[r][c]` is just equal to `matrix[r][c]`.
3. Accumulate and return the sum of all values in the DP matrix, as they represent individual squares.

### Code

```csharp
public class Solution {
    public int CountSquares(int[][] matrix) {
        int rows = matrix.Length;
        int cols = matrix[0].Length;
        int[][] dp = new int[rows][];
        for (int i = 0; i < rows; i++) {
            dp[i] = new int[cols];
        }
        int totalSquares = 0;

        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                // Only update dp array if cell is 1
                if (matrix[r][c] == 1) {
                    // For first row or first column cells, dp value is same as matrix[r][c]
                    if (r == 0 || c == 0) {
                        dp[r][c] = matrix[r][c];
                    } else {
                        // Minimum of left, top, and top-left diagonal plus the cell itself
                        dp[r][c] = Math.Min(dp[r-1][c], Math.Min(dp[r][c-1], dp[r-1][c-1])) + 1;
                    }
                    totalSquares += dp[r][c];
                }
            }
        }
        return totalSquares;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(m * n), where `m` is the number of rows and `n` is the number of columns. Each cell is processed once.
- **Space Complexity**: O(m * n), as we maintain a DP matrix of the same size as the input matrix. However, this can be optimized to O(n) by using a single-row DP array.

The dynamic programming approach is significantly more efficient in terms of time complexity compared to the brute force method and is generally the preferred solution for larger matrices.

