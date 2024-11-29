# [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

## Approach 1: Using Additional Arrays (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(m * n)
// Space Complexity: O(m + n)
class Solution {
    setZeroes(matrix: number[][]): void {
        const rows = matrix.length;
        const cols = matrix[0].length;

        const rowZero: boolean[] = new Array(rows).fill(false);
        const colZero: boolean[] = new Array(cols).fill(false);

        // Record rows and columns that need to be zeroed
        for (let i = 0; i < rows; i++) {
            for (let j = 0; j < cols; j++) {
                if (matrix[i][j] === 0) {
                    rowZero[i] = true;
                    colZero[j] = true;
                }
            }
        }

        // Set rows to zero
        for (let i = 0; i < rows; i++) {
            if (rowZero[i]) {
                for (let j = 0; j < cols; j++) {
                    matrix[i][j] = 0;
                }
            }
        }

        // Set columns to zero
        for (let j = 0; j < cols; j++) {
            if (colZero[j]) {
                for (let i = 0; i < rows; i++) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

## Approach 2: Using First Row and Column as Markers (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(m * n)
// Space Complexity: O(1)
class Solution {
    setZeroes(matrix: number[][]): void {
        const rows = matrix.length;
        const cols = matrix[0].length;
        let firstRowZero = false;
        let firstColZero = false;

        // Check if first row needs to be zeroed
        for (let j = 0; j < cols; j++) {
            if (matrix[0][j] === 0) {
                firstRowZero = true;
                break;
            }
        }

        // Check if first column needs to be zeroed
        for (let i = 0; i < rows; i++) {
            if (matrix[i][0] === 0) {
                firstColZero = true;
                break;
            }
        }

        // Use first row and column as markers
        for (let i = 1; i < rows; i++) {
            for (let j = 1; j < cols; j++) {
                if (matrix[i][j] === 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        // Set elements to zero based on markers
        for (let i = 1; i < rows; i++) {
            for (let j = 1; j < cols; j++) {
                if (matrix[i][0] === 0 || matrix[0][j] === 0) {
                    matrix[i][j] = 0;
                }
            }
        }

        // Zero out the first row if needed
        if (firstRowZero) {
            for (let j = 0; j < cols; j++) {
                matrix[0][j] = 0;
            }
        }

        // Zero out the first column if needed
        if (firstColZero) {
            for (let i = 0; i < rows; i++) {
                matrix[i][0] = 0;
            }
        }
    }
}
```

