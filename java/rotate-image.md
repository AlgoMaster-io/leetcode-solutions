# [Leetcode 48: Rotate Image](https://leetcode.com/problems/rotate-image/)

## Approaches

1. [Approach 1: Brute Force with Extra Space](#approach-1-brute-force-with-extra-space)
2. [Approach 2: In-Place Transpose and Reverse](#approach-2-in-place-transpose-and-reverse)

### Approach 1: Brute Force with Extra Space

**Intuition:**

To rotate a matrix clockwise by 90 degrees, we can utilize an auxiliary matrix. The idea is to iterate through each element of the matrix and place it appropriately in the new matrix. Specifically, each element at position `(i, j)` in the original matrix will be placed at position `(j, n-1-i)` in the rotated matrix.

**Algorithm:**

1. Create an auxiliary matrix of the same size as the input matrix.
2. For each element in the original matrix, calculate its new position in the auxiliary matrix using the formula: `rotated[j][n-1-i] = original[i][j]`.
3. Copy the rotated matrix back to the original matrix.

**Java Code:**

```java
public void rotate(int[][] matrix) {
    int n = matrix.length;
    // Create an auxiliary matrix to hold the rotated form
    int[][] rotated = new int[n][n];
    
    // Fill the rotated matrix by rotating elements clockwise
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            rotated[j][n - 1 - i] = matrix[i][j];
        }
    }
    
    // Copy the rotated matrix back to the original matrix
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            matrix[i][j] = rotated[i][j];
        }
    }
}
```

**Time Complexity:** O(n^2), where `n` is the number of rows (or columns) in the matrix.

**Space Complexity:** O(n^2) for the auxiliary matrix.

---

### Approach 2: In-Place Transpose and Reverse

**Intuition:**

A more efficient solution is to perform the transformation in place by first transposing the matrix (flipping it over its diagonal) and then reversing each row. Transposing by itself will reorder the elements diagonally, while reversing will yield the required 90-degree clockwise rotation.

**Algorithm:**

1. **Transpose the Matrix:** Swap each element `(i, j)` with the element `(j, i)`.
2. **Reverse Each Row:** For each row in the transposed matrix, reverse the elements of the row.

**Java Code:**

```java
public void rotate(int[][] matrix) {
    int n = matrix.length;
    
    // Transpose the matrix
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            // Swap elements on the diagonal
            int temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }
    
    // Reverse each row
    for (int i = 0; i < n; i++) {
        int start = 0;
        int end = n - 1;
        while (start < end) {
            // Swap elements to reverse the row
            int temp = matrix[i][start];
            matrix[i][start] = matrix[i][end];
            matrix[i][end] = temp;
            start++;
            end--;
        }
    }
}
```

**Time Complexity:** O(n^2), where `n` is the number of rows (or columns) in the matrix.

**Space Complexity:** O(1), since we are doing the operations in place without using extra space.

This method is optimal in terms of space utilization and computational overhead, providing an in-place rotation of the given matrix.

