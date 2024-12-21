# [LeetCode 73: Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized with O(m+n) Space](#optimized-with-om+n-space)
3. [Optimal In-Place Solution](#optimal-in-place-solution)

## Brute Force Approach

### Intuition
The naive approach involves having an auxiliary matrix of the same size as the input matrix. Whenever a zero is found in the input matrix, traverse through the entire row and column and mark their corresponding positions in the auxiliary matrix. Finally, update the input matrix using this auxiliary matrix.

### Steps
1. Create a matrix `temp` of the same size as `matrix` initialized with the values of `matrix`.
2. Traverse every element of `matrix`.
3. If an element is `0`, mark the entire row and column of `temp` corresponding to this `0` in `matrix`.
4. Copy the `temp` matrix back to `matrix`.

### Code
```javascript
function setZeroes(matrix) {
    const m = matrix.length;
    const n = matrix[0].length;
    const temp = matrix.map(row => [...row]);

    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            // If we find a zero in the original matrix, we mark 
            // the entire row and column in the temp matrix
            if (matrix[i][j] === 0) {
                for (let row = 0; row < m; row++) {
                    temp[row][j] = 0;
                }
                for (let col = 0; col < n; col++) {
                    temp[i][col] = 0;
                }
            }
        }
    }

    // Copy back the results to the original matrix
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            matrix[i][j] = temp[i][j];
        }
    }
}
```

### Complexity
- **Time Complexity**: O(m * n * (m + n)) due to multiple full row and column traversals.
- **Space Complexity**: O(m * n) due to the additional `temp` matrix.

## Optimized with O(m+n) Space 

### Intuition
We don't need another full matrix. We can use two arrays to store the row and column indices that should be set to zero.

### Steps
1. Use two arrays, `rows` and `cols`, to keep track of which rows and columns need to be zeroed.
2. First, traverse the matrix and record zero row and column indices.
3. Traverse through the matrix again and set the appropriate rows and columns to zero using the `rows` and `cols` arrays.

### Code
```javascript
function setZeroes(matrix) {
    const m = matrix.length;
    const n = matrix[0].length;
    const rows = Array(m).fill(false);
    const cols = Array(n).fill(false);

    // Step 1: Identify zero rows and zero columns
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (matrix[i][j] === 0) {
                rows[i] = true;
                cols[j] = true;
            }
        }
    }

    // Step 2: Set matrix elements to zero
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (rows[i] || cols[j]) {
                matrix[i][j] = 0;
            }
        }
    }
}
```

### Complexity
- **Time Complexity**: O(m * n) for two passes over the matrix.
- **Space Complexity**: O(m + n) for the additional `rows` and `cols` arrays.

## Optimal In-Place Solution

### Intuition
Use the first row and the first column of the matrix itself to keep track of which row or column need to be zeroed. This allows us to reduce the space complexity to O(1), besides the input space.

### Steps
1. Determine whether the first row and/or first column need to be zeroed initially.
2. Use the first row and column to mark zeros found in the rest of the matrix.
3. Traverse the matrix, using the markings to set elements to zero appropriately.
4. Finally set the first row and/or column to zero if they were initially marked.

### Code
```javascript
function setZeroes(matrix) {
    const m = matrix.length;
    const n = matrix[0].length;
    let firstRowZero = false;
    let firstColZero = false;

    // Check if first row needs to be zero
    for (let j = 0; j < n; j++) {
        if (matrix[0][j] === 0) {
            firstRowZero = true;
            break;
        }
    }

    // Check if first column needs to be zero
    for (let i = 0; i < m; i++) {
        if (matrix[i][0] === 0) {
            firstColZero = true;
            break;
        }
    }

    // Use the first row and column to mark zero positions
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (matrix[i][j] === 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }

    // Zero out cells based on marks
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (matrix[i][0] === 0 || matrix[0][j] === 0) {
                matrix[i][j] = 0;
            }
        }
    }

    // Zero out the first row if needed
    if (firstRowZero) {
        for (let j = 0; j < n; j++) {
            matrix[0][j] = 0;
        }
    }

    // Zero out the first column if needed
    if (firstColZero) {
        for (let i = 0; i < m; i++) {
            matrix[i][0] = 0;
        }
    }
}
```

### Complexity
- **Time Complexity**: O(m * n) for a constant number of matrix scans.
- **Space Complexity**: O(1) since we use no additional space except the input matrix.

