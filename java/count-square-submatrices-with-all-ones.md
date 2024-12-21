# [LeetCode 1277: Count Square Submatrices with All Ones](https://leetcode.com/problems/count-square-submatrices-with-all-ones/)

## Approaches
1. [Brute Force Solution](#brute-force-solution)
2. [Dynamic Programming Solution](#dynamic-programming-solution)

### Brute Force Solution

**Intuition:**

The basic idea is to count all possible square submatrices in the matrix that have all ones. The brute force approach involves iterating over every potential square submatrix and checking if it consists entirely of ones.

- Start at every element in the matrix, try expanding it into a square submatrix.
- Check each submatrix to see if every entry is `1`.
- If a square is valid, increase the count.

This approach checks every possible square matrix, and hence can be quite inefficient for larger matrices.

**Code:**

```java
public int countSquares(int[][] matrix) {
    int m = matrix.length;
    int n = matrix[0].length;
    int count = 0;

    // iterate over each potential top-left corner of a square
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            // maximum size of square that can be possibly formed with (i, j) as top-left corner
            int maxSize = Math.min(m - i, n - j);
            // consider all squares from size 1 to maxSize
            for (int size = 1; size <= maxSize; size++) {
                if (isAllOnes(matrix, i, j, size)) {
                    count++;
                } else {
                    break;  // no need to check larger squares if current one is not valid
                }
            }
        }
    }

    return count;
}

// Check if all elements in the submatrix of given size starting at (startX, startY) are ones
private boolean isAllOnes(int[][] matrix, int startX, int startY, int size) {
    for (int i = startX; i < startX + size; i++) {
        for (int j = startY; j < startY + size; j++) {
            if (matrix[i][j] != 1) {
                return false;
            }
        }
    }
    return true;
}
```

**Complexity Analysis:**

- Time Complexity: \(O(m \times n \times \min(m, n)^2)\), where \(m\) and \(n\) are the dimensions of the matrix. We check each submatrix for validity up to the possible maximum size.
- Space Complexity: \(O(1)\), since we are using no extra space apart from variables.

### Dynamic Programming Solution

**Intuition:**

The dynamic programming approach incrementally builds a solution using previous results. The key insight is recognizing that the size of the square submatrix ending at position `(i, j)` can be determined by the smallest square ending at positions `(i-1, j)`, `(i, j-1)`, and `(i-1, j-1)`, if `(i, j)` itself is `1`.

- Create a DP table where `dp[i][j]` represents the size of the largest square submatrix with all ones ending at `(i, j)`.
- Update this table iteratively based on the above relation.
- Sum up all the entries in the DP table to get the total count of squares.

**Code:**

```java
public int countSquares(int[][] matrix) {
    int m = matrix.length;
    int n = matrix[0].length;
    int[][] dp = new int[m][n];
    int count = 0;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            // Base case: First row or first column
            if (i == 0 || j == 0) {
                dp[i][j] = matrix[i][j];
            } else if (matrix[i][j] == 1) {
                // Determine min square size possible
                dp[i][j] = Math.min(dp[i - 1][j], Math.min(dp[i][j - 1], dp[i - 1][j - 1])) + 1;
            }
            // Add to total count
            count += dp[i][j];
        }
    }

    return count;
}
```

**Complexity Analysis:**

- Time Complexity: \(O(m \times n)\), where \(m\) and \(n\) are the dimensions of the matrix. Each cell is processed once.
- Space Complexity: \(O(m \times n)\), additional space used for the DP matrix. However, this can be optimized to \(O(n)\) since we only need the current and previous row.

In summary, the dynamic programming solution significantly enhances efficiency over the brute force approach, particularly suited for larger matrix sizes.

