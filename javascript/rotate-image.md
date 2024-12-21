# [Leetcode Problem 48: Rotate Image](https://leetcode.com/problems/rotate-image/)

## Approaches
1. [Naive Approach using Extra Space](#naive-approach-using-extra-space)
2. [In-Place Transpose and Reverse](#in-place-transpose-and-reverse)

---

### Naive Approach using Extra Space

#### Intuition
The simplest way to solve this problem is by leveraging an extra matrix to store the rotated image. For a given N x N matrix, the element at position `(i, j)` will be moved to position `(j, N-1-i)` in a new matrix, effectively performing a 90-degree clockwise rotation.

#### Steps
1. Create an empty N x N matrix `rotated`.
2. Iterate over each element `(i, j)` in the original matrix:
   - Place the element at position `(i, j)` into `rotated[j][N-i-1]`.
3. Copy the `rotated` matrix back to the original matrix to reflect the rotation.

#### Code
```javascript
function rotate(matrix) {
    const n = matrix.length;
    // Create a new matrix of the same size
    const rotated = new Array(n).fill(null).map(() => new Array(n).fill(0));

    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            // Place element at new position in rotated matrix
            rotated[j][n - i - 1] = matrix[i][j];
        }
    }

    // Copy the rotated matrix back to the original matrix
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            matrix[i][j] = rotated[i][j];
        }
    }
}
```

#### Complexity
- **Time Complexity:** `O(N^2)`, where N is the number of rows (or columns) in the matrix.
- **Space Complexity:** `O(N^2)`, due to the use of an additional matrix.

---

### In-Place Transpose and Reverse

#### Intuition
We can rotate the image in-place without using extra space by first transposing the matrix and then reversing each row. A transpose swaps the element at `(i, j)` with the element at `(j, i)`. After transposing, reversing each row will complete the 90-degree rotation.

#### Steps
1. **Transpose the Matrix**:
   - Swap the elements at `(i, j)` and `(j, i)` for all `i < j`.
2. **Reverse Each Row**:
   - Reverse each row of the transposed matrix to complete the rotation.

#### Code
```javascript
function rotate(matrix) {
    const n = matrix.length;

    // Transpose the matrix
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            // Swap elements
            [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
        }
    }
    
    // Reverse each row
    for (let i = 0; i < n; i++) {
        matrix[i].reverse();
    }
}
```

#### Complexity
- **Time Complexity:** `O(N^2)`, since we have to visit each element twice (once for transpose and once for reverse).
- **Space Complexity:** `O(1)`, as we're doing the operations in-place with no extra memory. 

By applying these methods, we effectively rotate a given N x N matrix 90 degrees clockwise using both extra space and in-place solutions.

