# [Leetcode 48: Rotate Image](https://leetcode.com/problems/rotate-image/)

## Approach
1. [Brute Force Approach](#brute-force-approach)
2. [Transpose and Reverse Approach](#transpose-and-reverse-approach)

### Brute Force Approach

#### Intuition
The problem requires us to rotate an `n x n` 2D matrix by 90 degrees to the right (clockwise). A direct approach is to create a new matrix and place elements in their new positions. This involves iterating over the original matrix, taking each element, and placing it into the new matrix at the rotated position.

#### Steps
- Create a new matrix of the same size initialized with zeroes.
- For each element `matrix[i][j]`, place it in the new matrix at position `[j][n-i-1]`.
- Copy the new matrix back to the original matrix.

#### Time Complexity
- **O(n^2)**: We traverse each element in the matrix once.

#### Space Complexity
- **O(n^2)**: We use additional space for the new matrix.

```typescript
function rotateBruteForce(matrix: number[][]): void {
    const n = matrix.length;
    const newMatrix: number[][] = Array.from({ length: n }, () => Array(n).fill(0));

    // Place the elements in the new matrix based on the rotation
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            newMatrix[j][n - 1 - i] = matrix[i][j];
        }
    }

    // Copy the new matrix back to the original matrix
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            matrix[i][j] = newMatrix[i][j];
        }
    }
}
```

### Transpose and Reverse Approach

#### Intuition
An optimal approach with better space complexity is to first transpose the matrix and then reverse each row. Transposing involves converting all rows to columns and vice versa, and reversing each row results in the required rotation.

#### Steps
1. **Transpose the Matrix**: Swap all rows and columns so that `matrix[i][j]` becomes `matrix[j][i]`.
2. **Reverse Each Row**: Reverse the elements of each row to complete the 90-degree rotation.

#### Time Complexity
- **O(n^2)**: Transposing and reversing each row involves iterating over each element once.

#### Space Complexity
- **O(1)**: The rotation is done in place with no additional matrix space used.

```typescript
function rotate(matrix: number[][]): void {
    const n = matrix.length;

    // Transpose the matrix
    for (let i = 0; i < n; i++) {
        for (let j = i; j < n; j++) {
            // Swap elements to transpose
            const temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }

    // Reverse each row
    for (let i = 0; i < n; i++) {
        matrix[i].reverse();
    }
}
```

Both approaches successfully solve the problem. The second approach is more space efficient and is preferable in scenarios where matrix size is large and memory is a concern.

