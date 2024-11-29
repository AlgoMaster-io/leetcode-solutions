# [48. Rotate Image](https://leetcode.com/problems/rotate-image/)

## Approach 1: Transpose and Reverse Rows (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
function rotate(matrix: number[][]): void {
    const n = matrix.length;

    // Step 1: Transpose the matrix
    for (let i = 0; i < n; i++) {
        for (let j = i; j < n; j++) {
            const temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }

    // Step 2: Reverse each row
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < Math.floor(n / 2); j++) {
            const temp = matrix[i][j];
            matrix[i][j] = matrix[i][n - j - 1];
            matrix[i][n - j - 1] = temp;
        }
    }
}
```

## Approach 2: Rotating Layer by Layer (In-Place)

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
function rotate(matrix: number[][]): void {
    const n = matrix.length;

    // Rotate each layer
    for (let layer = 0; layer < Math.floor(n / 2); layer++) {
        const first = layer;
        const last = n - layer - 1;

        for (let i = first; i < last; i++) {
            const offset = i - first;

            // Save top element
            const top = matrix[first][i];

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
```

