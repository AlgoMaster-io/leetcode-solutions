# [Leetcode 48: Rotate Image](https://leetcode.com/problems/rotate-image)

## Approaches
1. [Transpose and Reverse each row](#transpose-and-reverse-each-row)
2. [Layer by Layer Rotation](#layer-by-layer-rotation)

---

### 1. Transpose and Reverse each row

The basic idea to solve this problem is to perform two operations:
- **Transpose**: Convert the rows of the matrix into columns, basically swap `matrix[i][j]` with `matrix[j][i]`.
- **Reverse each row**: After transposing, reverse each individual row to achieve a 90-degree rotation.

**Intuition:**

By transposing the matrix, we essentially flip it over its diagonal (top-left to bottom-right), which makes the rows of the original matrix turn into columns. To achieve the clockwise 90-degree rotation, we simply reverse each row, which effectively mirrors the elements over the vertical axis. This is a common approach due to its simplicity and efficiency.

```go
func rotate(matrix [][]int) {
    n := len(matrix)
    
    // Transpose the matrix
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            // Swap matrix[i][j] and matrix[j][i]
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
        }
    }
    
    // Reverse each row
    for i := 0; i < n; i++ {
        for j := 0; j < n/2; j++ {
            // Swap the elements to reverse the row
            matrix[i][j], matrix[i][n-j-1] = matrix[i][n-j-1], matrix[i][j]
        }
    }
}
```

**Time Complexity:** O(n^2) - We pass through the entire matrix twice: once for transposing and once for reversing each row.

**Space Complexity:** O(1) - The rotation is done in-place, so no additional space is used.

---

### 2. Layer by Layer Rotation

Another approach is to rotate the outer layer of the matrix, move inward layer by layer.

**Intuition:**

For a matrix of size n x n, consider the outermost layer as containing the four sides of the matrix. We rotate these sides clockwise. We repeat the same for the inner layers until the entire matrix is rotated. We move the four numbers in `n^2 - 1` places and shift them.

```go
func rotate(matrix [][]int) {
    n := len(matrix)
    
    // Layer by layer rotation
    for layer := 0; layer < n / 2; layer++ {
        first := layer
        last := n - 1 - layer
        for i := first; i < last; i++ {
            offset := i - first
            top := matrix[first][i] // save top element
            
            // left -> top
            matrix[first][i] = matrix[last-offset][first]
            
            // bottom -> left
            matrix[last-offset][first] = matrix[last][last-offset]
            
            // right -> bottom
            matrix[last][last-offset] = matrix[i][last]
            
            // top -> right
            matrix[i][last] = top
        }
    }
}
```

**Time Complexity:** O(n^2) - Despite the approach looking more mechanical, it still involves iterating over elements of the matrix, rotating each element once.

**Space Complexity:** O(1) - Just like the previous method, this approach also rotates the matrix in-place, requiring no additional space for rotation.

