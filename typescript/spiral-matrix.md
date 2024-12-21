# [Leetcode 54: Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Approaches

- [Approach 1: Iterative Spiral Traversal](#approach-1-iterative-spiral-traversal)
- [Approach 2: Layer by Layer Stripping](#approach-2-layer-by-layer-stripping)

---

## Approach 1: Iterative Spiral Traversal

### Intuition

The task is to return all elements of a given m x n matrix in spiral order. The intuitive way to achieve this is by using a series of loops to traverse the matrix layer by layer. We maintain four variables (`top`, `bottom`, `left`, and `right`) that represent the current boundaries of the matrix. We start from the top-left corner and iteratively update these bounds as we proceed to traverse the matrix in a spiral manner.

### Algorithm

1. Start by initializing the boundary markers: `top` to 0, `bottom` to `matrix.length - 1`, `left` to 0, and `right` to `matrix[0].length - 1`.
2. Loop through the matrix until `top` exceeds `bottom` or `left` exceeds `right`.
3. For each layer:
   - Traverse from left to right across the `top` row, then increment `top`.
   - Traverse from top to bottom along the `right` column, then decrement `right`.
   - If `top` <= `bottom`, traverse from right to left across the `bottom` row, then decrement `bottom`.
   - If `left` <= `right`, traverse from bottom to top along the `left` column, then increment `left`.
4. Continue this process until all elements have been added to the result.

### Time and Space Complexity

- **Time Complexity:** O(m * n), where `m` is the number of rows and `n` is the number of columns, since we process each element exactly once.
- **Space Complexity:** O(1), not counting the space required for the output.

### Code

```typescript
function spiralOrder(matrix: number[][]): number[] {
    const result: number[] = [];
    if (matrix.length === 0) return result;
    
    let top = 0, bottom = matrix.length - 1;
    let left = 0, right = matrix[0].length - 1;
    
    while (top <= bottom && left <= right) {
        // Traverse from left to right along the top row
        for (let i = left; i <= right; i++) {
            result.push(matrix[top][i]);
        }
        top++;
        
        // Traverse from top to bottom along the right column
        for (let i = top; i <= bottom; i++) {
            result.push(matrix[i][right]);
        }
        right--;
        
        if (top <= bottom) {
            // Traverse from right to left along the bottom row
            for (let i = right; i >= left; i--) {
                result.push(matrix[bottom][i]);
            }
            bottom--;
        }
        
        if (left <= right) {
            // Traverse from bottom to top along the left column
            for (let i = bottom; i >= top; i--) {
                result.push(matrix[i][left]);
            }
            left++;
        }
    }
    
    return result;
}
```

---

## Approach 2: Layer by Layer Stripping

### Intuition

Instead of using dynamic boundaries to track the current layer of the matrix, this approach strips layers of the matrix one by one and adds the elements in spiral order. This method highlights the recursive nature of the problem by iteratively removing the outer layer and recursively processing the inner matrix.

### Algorithm

1. Declare a helper function to process one layer at a time:
   - If the matrix is not empty, process the top row, right column, bottom row, and left column, adding elements to the result list.
   - Recursively invoke the helper with the submatrix (`matrix.slice(1, -1).map(row => row.slice(1, -1))`).
2. Invoke the helper function with the original matrix and collect all elements in spiral order.

### Time and Space Complexity

- **Time Complexity:** O(m * n) because each element is processed exactly once.
- **Space Complexity:** O(m * n), due to the creation of sliced submatrices at each recursive step.

### Code

```typescript
function spiralOrderLayerByLayer(matrix: number[][]): number[] {
    const result: number[] = [];
    
    const helper = (mat: number[][]): void => {
        if (mat.length === 0) return;
        
        // Add the top row
        result.push(...mat[0]);
        
        // Add the right column
        for (let i = 1; i < mat.length - 1; i++) {
            if (mat[i].length) {
                result.push(mat[i][mat[i].length - 1]);
            }
        }
        
        // Add the bottom row if it's different from the top row
        if (mat.length > 1) {
            const bottomRow = mat[mat.length - 1];
            result.push(...bottomRow.reverse());
        }
        
        // Add the left column
        for (let i = mat.length - 2; i > 0; i--) {
            if (mat[i].length) {
                result.push(mat[i][0]);
            }
        }
        
        // Recur for the inner layers
        const nextLayer = mat.slice(1, -1).map(row => row.slice(1, -1));
        helper(nextLayer);
    };
    
    helper(matrix);
    return result;
}
```

Approach 1 is typically more efficient in terms of space usage, while Approach 2 nicely demonstrates the recursive structure that can be used in similar problems. Both effectively solve the problem and demonstrate how to handle matrix traversal in a non-linear fashion.

