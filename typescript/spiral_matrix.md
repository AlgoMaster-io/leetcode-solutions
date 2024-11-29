# [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Approach 1: Simulation with Boundaries (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(m * n)
// Space Complexity: O(1) (excluding output list)

function spiralOrder(matrix: number[][]): number[] {
    const result: number[] = [];

    if (matrix.length === 0) return result;

    let top = 0, bottom = matrix.length - 1;
    let left = 0, right = matrix[0].length - 1;

    while (top <= bottom && left <= right) {
        // Traverse from left to right
        for (let i = left; i <= right; i++) {
            result.push(matrix[top][i]);
        }
        top++;

        // Traverse from top to bottom
        for (let i = top; i <= bottom; i++) {
            result.push(matrix[i][right]);
        }
        right--;

        // Traverse from right to left
        if (top <= bottom) {
            for (let i = right; i >= left; i--) {
                result.push(matrix[bottom][i]);
            }
            bottom--;
        }

        // Traverse from bottom to top
        if (left <= right) {
            for (let i = bottom; i >= top; i--) {
                result.push(matrix[i][left]);
            }
            left++;
        }
    }

    return result;
}
```

## Approach 2: Simulated Layer Traversal (Alternative)

### Solution
typescript
```typescript
// Time Complexity: O(m * n)
// Space Complexity: O(1) (excluding output list)

function spiralOrder(matrix: number[][]): number[] {
    const result: number[] = [];

    if (matrix.length === 0) return result;

    let rows = matrix.length;
    let cols = matrix[0].length;
    let startRow = 0, startCol = 0;

    while (startRow < rows && startCol < cols) {
        // Traverse the top row
        for (let i = startCol; i < cols; i++) {
            result.push(matrix[startRow][i]);
        }
        startRow++;

        // Traverse the right column
        for (let i = startRow; i < rows; i++) {
            result.push(matrix[i][cols - 1]);
        }
        cols--;

        // Traverse the bottom row
        if (startRow < rows) {
            for (let i = cols - 1; i >= startCol; i--) {
                result.push(matrix[rows - 1][i]);
            }
            rows--;
        }

        // Traverse the left column
        if (startCol < cols) {
            for (let i = rows - 1; i >= startRow; i--) {
                result.push(matrix[i][startCol]);
            }
            startCol++;
        }
    }

    return result;
}
```

