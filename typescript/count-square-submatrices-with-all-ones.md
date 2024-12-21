[Leetcode 1277: Count Square Submatrices with All Ones](https://leetcode.com/problems/count-square-submatrices-with-all-ones/)

## Approaches
1. [Brute Force Solution](#brute-force-solution)
2. [Dynamic Programming Solution](#dynamic-programming-solution)

### Brute Force Solution

#### Intuition
The idea in the brute force approach is to iterate over every possible top-left corner of a square submatrix made of 1s in the given matrix. For each such potential top-left corner, we attempt to form a square and check if all the elements within the square are 1s. We start with the smallest possible square (1x1) and expand to larger squares incrementally, counting each valid square formed.

#### Code
```typescript
function countSquares(matrix: number[][]): number {
    let totalCount = 0;
    const rows = matrix.length;
    const cols = matrix[0].length;

    // Iterate over every cell in the matrix as a potential top-left corner of the square
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            // Initially consider only 1x1 square
            let maxSize = Math.min(rows - i, cols - j);
            for (let size = 1; size <= maxSize; size++) {
                let isSquare = true;
                // Check if we can form a square of current size starting at (i,j)
                for (let x = 0; x < size; x++) {
                    for (let y = 0; y < size; y++) {
                        if (matrix[i + x][j + y] !== 1) {
                            isSquare = false;
                            break;
                        }
                    }
                    if (!isSquare) break;
                }
                if (isSquare) {
                    totalCount++;
                } else {
                    break;
                }
            }
        }
    }

    return totalCount;
}
```

#### Time Complexity
- **O((m*n)^2)**, where m is the number of rows and n is the number of columns in the matrix. For each cell in the matrix, we check up to the largest possible square.
   
#### Space Complexity
- **O(1)**, as we're using only a constant amount of extra space.

### Dynamic Programming Solution

#### Intuition
A more optimal approach utilizes dynamic programming by building up the solution using previously computed results. We use a dp table where `dp[i][j]` represents the size of the largest square submatrix with all ones ending at position `(i, j)`. The value of `dp[i][j]` is determined based on the values directly above, to the left, and diagonally above-left. If `matrix[i][j]` is 1, then `dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])`. This allows us to efficiently calculate all square submatrices and their sizes.

#### Code
```typescript
function countSquares(matrix: number[][]): number {
    const rows = matrix.length;
    const cols = matrix[0].length;
    let totalSquares = 0;
    
    // Use the same matrix due to space saving; alternatively, a new dp table can also be used.
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            // If the current cell is 1 and it is not in the first row or first column
            if (matrix[i][j] === 1) {
                if (i > 0 && j > 0) {
                    matrix[i][j] = Math.min(matrix[i-1][j], matrix[i][j-1], matrix[i-1][j-1]) + 1;
                }
                totalSquares += matrix[i][j];
            }
        }
    }

    return totalSquares;
}
```

#### Time Complexity
- **O(m*n)**, since every cell in the matrix is processed once.

#### Space Complexity
- **O(1)**, reusing the input matrix for storing DP values. Alternatively, with a new dp matrix, it would be **O(m*n)**.

