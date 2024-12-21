# [LeetCode 54: Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Approaches:
1. [Iterative Layer-by-Layer Traversal](#approach-1-iterative-layer-by-layer-traversal)

### Approach 1: Iterative Layer-by-Layer Traversal

**Intuition:**

To retrieve elements in a spiral order from a matrix, we can think of virtually peeling off layers from the matrix one at a time, starting from the outermost layer and moving inwards. Each layer essentially is made up of four directions:
- Left to Right (Top row)
- Top to Bottom (Right column)
- Right to Left (Bottom row)
- Bottom to Top (Left column)

By maintaining boundary pointers for these rows and columns, we sequentially extract these "layers" until there are no more left to extract.

**Detailed Steps:**

1. Define boundary pointers for rows (`top`, `bottom`) and columns (`left`, `right`).
2. Initialize a result list, which will store our spiral order.
3. Loop through each direction:
   - **Left to Right**: Iterate from `left` to `right` on the `top` row. Increase the `top` boundary after visiting the row.
   - **Top to Bottom**: Iterate downwards from `top` to `bottom` on the `right` column. Decrease the `right` boundary after visiting the column.
   - **Right to Left**: Ensure we are still within boundaries. Iterate from `right` to `left` on the `bottom` row. Decrease the `bottom` boundary after visiting the row.
   - **Bottom to Top**: Ensure we are still within boundaries. Iterate upwards from `bottom` to `top` on the `left` column. Increase the `left` boundary after visiting the column.
4. Continue this process until `top` is greater than `bottom` or `left` is greater than `right`.

**Code Implementation:**

```javascript
function spiralOrder(matrix) {
    // Corner case for an empty matrix
    if (matrix.length === 0 || matrix[0].length === 0) return [];
    
    const result = [];
    let top = 0;
    let bottom = matrix.length - 1;
    let left = 0;
    let right = matrix[0].length - 1;
    
    while (top <= bottom && left <= right) {
        // Traverse from left to right along the top boundary
        for (let i = left; i <= right; i++) {
            result.push(matrix[top][i]);
        }
        top++;
        
        // Traverse from top to bottom along the right boundary
        for (let i = top; i <= bottom; i++) {
            result.push(matrix[i][right]);
        }
        right--;
        
        // Check row boundary before traversing from right to left
        if (top <= bottom) {
            for (let i = right; i >= left; i--) {
                result.push(matrix[bottom][i]);
            }
            bottom--;
        }
        
        // Check column boundary before traversing from bottom to top
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

**Time Complexity:** O(m * n), where `m` is the number of rows and `n` is the number of columns in the matrix. Each element is visited once.

**Space Complexity:** O(1), ignoring the space required for the output list. The function uses a constant amount of extra space.

