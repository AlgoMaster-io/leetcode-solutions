# [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach Using Additional Data Structures](#optimized-approach-using-additional-data-structures)
3. [Most Optimal Approach: In-place Modification](#most-optimal-approach-in-place-modification)

---

### Brute Force Approach

**Intuition**
The brute force solution involves creating a temporary matrix that will be used to store the state of the matrix after setting the appropriate rows and columns to zero, while iterating through the input matrix. We first mark which rows and columns need to be set to zero, and then we apply the transformation.

```typescript
function setZeroes(matrix: number[][]): void {
    const rows = matrix.length;
    const cols = matrix[0].length;
    const temp = new Array(rows).fill(0).map(() => new Array(cols).fill(0));

    // Copy the matrix to temp
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            temp[r][c] = matrix[r][c];
        }
    }

    // Mark rows and columns to be zeroed
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (matrix[r][c] === 0) {
                // Set the entire row in temp to 0
                for (let col = 0; col < cols; col++) {
                    temp[r][col] = 0;
                }
                // Set the entire column in temp to 0
                for (let row = 0; row < rows; row++) {
                    temp[row][c] = 0;
                }
            }
        }
    }

    // Copy the temp back to the matrix
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            matrix[r][c] = temp[r][c];
        }
    }
}
```

**Time Complexity**: O(m*n), where m is the number of rows and n is the number of columns.  
**Space Complexity**: O(m*n), due to the temporary matrix.

---

### Optimized Approach Using Additional Data Structures

**Intuition**
We use two sets to track rows and columns that need to be zeroed. The conditions are checked once, and the resulting zeroing operation runs in linear time for each row and column tracked.

```typescript
function setZeroes(matrix: number[][]): void {
    const rows = matrix.length;
    const cols = matrix[0].length;
    const zeroRows = new Set<number>();
    const zeroCols = new Set<number>();

    // Mark rows and columns that need to be zeroed
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (matrix[r][c] === 0) {
                zeroRows.add(r);
                zeroCols.add(c);
            }
        }
    }

    // Zero out marked rows
    for (let r of zeroRows) {
        for (let c = 0; c < cols; c++) {
            matrix[r][c] = 0;
        }
    }

    // Zero out marked columns
    for (let c of zeroCols) {
        for (let r = 0; r < rows; r++) {
            matrix[r][c] = 0;
        }
    }
}
```

**Time Complexity**: O(m*n), where m is the number of rows and n is the number of columns.  
**Space Complexity**: O(m + n), due to additional structures used to store row and column indices.

---

### Most Optimal Approach: In-place Modification

**Intuition**
We can achieve constant space usage by using the first row and first column as markers for which rows and columns need to be zeroed. This approach requires careful management of the state when the first row or column itself needs to be zeroed.

```typescript
function setZeroes(matrix: number[][]): void {
    const rows = matrix.length;
    const cols = matrix[0].length;
    let firstRowZero = false;
    let firstColZero = false;

    // Determine if the first row or column needs to be zeroed
    for (let r = 0; r < rows; r++) {
        if (matrix[r][0] === 0) {
            firstColZero = true;
            break;
        }
    }

    for (let c = 0; c < cols; c++) {
        if (matrix[0][c] === 0) {
            firstRowZero = true;
            break;
        }
    }

    // Use first row and first column to mark zeroes
    for (let r = 1; r < rows; r++) {
        for (let c = 1; c < cols; c++) {
            if (matrix[r][c] === 0) {
                matrix[r][0] = 0;
                matrix[0][c] = 0;
            }
        }
    }

    // Zero the marked rows and columns
    for (let r = 1; r < rows; r++) {
        for (let c = 1; c < cols; c++) {
            if (matrix[r][0] === 0 || matrix[0][c] === 0) {
                matrix[r][c] = 0;
            }
        }
    }

    // Zero the first row if needed
    if (firstRowZero) {
        for (let c = 0; c < cols; c++) {
            matrix[0][c] = 0;
        }
    }

    // Zero the first column if needed
    if (firstColZero) {
        for (let r = 0; r < rows; r++) {
            matrix[r][0] = 0;
        }
    }
}
```

**Time Complexity**: O(m*n), where m is the number of rows and n is the number of columns.  
**Space Complexity**: O(1), as the operations are performed in-place using the matrix itself for storage.

