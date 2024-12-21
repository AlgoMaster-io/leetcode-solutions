[Leetcode 1277: Count Square Submatrices with All Ones](https://leetcode.com/problems/count-square-submatrices-with-all-ones/)

### Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)

### Brute Force Approach

The brute force approach involves iterating over every possible top-left corner of a submatrix and then checking if it's possible to create a square submatrix with `1`s of varying sizes starting from that position. This involves checking each possible size of square submatrix (1x1, 2x2, ..., nxn) and verifying that all values in the potential submatrix are `1`s.

**Intuition:**
- Start from every cell in the matrix as a potential top-left corner of a square.
- Try to form squares of increasing sizes from that position.
- For each square size, verify all its entries are `1`.
- Count all valid squares.

**Code Implementation:**

```javascript
var countSquares = function(matrix) {
    let totalSquares = 0;
    let rows = matrix.length;
    let cols = matrix[0].length;
    
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            // Try every possible square size
            for (let size = 1; i + size <= rows && j + size <= cols; size++) {
                let isSquare = true;
                // Check all elements in the current square
                for (let x = i; x < i + size; x++) {
                    for (let y = j; y < j + size; y++) {
                        if (matrix[x][y] !== 1) {
                            isSquare = false;
                            break;
                        }
                    }
                    if (!isSquare) break;
                }
                if (isSquare) totalSquares++;
            }
        }
    }
    
    return totalSquares;
};
```

**Time Complexity:** O(n^3), where n is the dimension of the matrix.  
**Space Complexity:** O(1), since no additional space is used apart from a few variables.

### Dynamic Programming Approach

The dynamic programming approach builds on sub-problems to avoid redundant calculations and achieve optimal performance.

**Intuition:**
- Use a DP table where `dp[i][j]` represents the maximum size square that can be formed with the bottom-right corner at position `(i, j)`.
- Base cases are the first row and first column, where `dp[i][j]` is simply the same as `matrix[i][j]` since larger squares cannot exist.
- For other cells: if the cell in the original matrix is `1`, the square size is 1 plus the minimum of the sizes of squares ending at `(i-1, j)`, `(i, j-1)`, and `(i-1, j-1)`.

**Code Implementation:**

```javascript
var countSquares = function(matrix) {
    let rows = matrix.length;
    let cols = matrix[0].length;
    let totalSquares = 0;
    
    // DP table: use the same matrix to save space as it contains binary values
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            // Only consider matrix[i][j] if it is `1`
            if (matrix[i][j] === 1 && i > 0 && j > 0) {
                matrix[i][j] = Math.min(matrix[i-1][j-1], matrix[i-1][j], matrix[i][j-1]) + 1;
            }
            // Accumulate the total number of squares
            totalSquares += matrix[i][j];
        }
    }
    
    return totalSquares;
};
```

**Time Complexity:** O(n^2), where n is the dimension of the matrix.  
**Space Complexity:** O(1), using the input matrix to store DP results. 

This dynamic programming solution efficiently counts all square submatrices with all `1`s, leveraging previous computations through the DP table adjacency properties.

