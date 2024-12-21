# [Leetcode 73: Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Using Additional Space for Row and Column Tracking](#using-additional-space-for-row-and-column-tracking)
3. [Optimal Approach Using Constant Space](#optimal-approach-using-constant-space)

## Brute Force Approach

### Intuition
In the brute force approach, we will iterate over the matrix to find the positions of all zeros. For each found zero, we will then fill the entire row and column with a special marker (different from zero to avoid confusion), indicating which positions need to become zeros in the next pass.

### Detailed Steps
1. Traverse the matrix to find all positions containing zero and temporarily mark their row and column with a special marker (e.g., a large negative number).
2. Traverse the matrix again, converting every cell marked with this special marker into zero.

### Code
```go
func setZeroes(matrix [][]int) {
    rows := len(matrix)
    cols := len(matrix[0])
    // Use an arbitrary large negative number as a marker
    marker := -1000000

    // First pass: mark the rows and columns which need to be zeroed
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if matrix[i][j] == 0 {
                // Mark entire row and column with the marker
                for k := 0; k < cols; k++ {
                    if matrix[i][k] != 0 {
                        matrix[i][k] = marker
                    }
                }
                for k := 0; k < rows; k++ {
                    if matrix[k][j] != 0 {
                        matrix[k][j] = marker
                    }
                }
            }
        }
    }

    // Second pass: convert markers to zero
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if matrix[i][j] == marker {
                matrix[i][j] = 0
            }
        }
    }
}
```

### Complexity
- **Time Complexity:** O(m * n * (m + n)), where m and n are the number of rows and columns, respectively.
- **Space Complexity:** O(1), additional space is used only for the marker.

## Using Additional Space for Row and Column Tracking

### Intuition
Instead of marking zero positions in place, use two additional arrays to track which rows and columns should be filled with zeros.

### Detailed Steps
1. Use two arrays: `rowsToZero` and `colsToZero` to record the rows and columns that must be zeroed.
2. Traverse the matrix to update these arrays.
3. Zero out all marked rows and columns based on these arrays.

### Code
```go
func setZeroes(matrix [][]int) {
    rows := len(matrix)
    cols := len(matrix[0])
    rowsToZero := make([]bool, rows)
    colsToZero := make([]bool, cols)

    // Record rows and columns that need to be zeroed
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if matrix[i][j] == 0 {
                rowsToZero[i] = true
                colsToZero[j] = true
            }
        }
    }

    // Set the corresponding rows to zero
    for i := 0; i < rows; i++ {
        if rowsToZero[i] {
            for j := 0; j < cols; j++ {
                matrix[i][j] = 0
            }
        }
    }

    // Set the corresponding columns to zero
    for j := 0; j < cols; j++ {
        if colsToZero[j] {
            for i := 0; i < rows; i++ {
                matrix[i][j] = 0
            }
        }
    }
}
```

### Complexity
- **Time Complexity:** O(m * n), where m and n are the number of rows and columns, respectively.
- **Space Complexity:** O(m + n) for the additional arrays.

## Optimal Approach Using Constant Space

### Intuition
Instead of using additional arrays, use the first row and column of the matrix itself for this purpose, with additional flags to track whether the first row and column need to be zeroed.

### Detailed Steps
1. Use two variables `rowZero` and `colZero` to determine if the first row or column needs to be zeroed.
2. Traverse and use the first row and column to record zero information for the other parts of the matrix.
3. Zero out all necessary rows and columns based on the marks in the first row and column.
4. Finally, handle zeroing of the first row and column based on the flags.

### Code
```go
func setZeroes(matrix [][]int) {
    rows := len(matrix)
    cols := len(matrix[0])
    rowZero, colZero := false, false
    
    // Determine if first row or column needs to be zeroed
    for i := 0; i < rows; i++ {
        if matrix[i][0] == 0 {
            colZero = true
            break
        }
    }
    for j := 0; j < cols; j++ {
        if matrix[0][j] == 0 {
            rowZero = true
            break
        }
    }
    
    // Use first row and column as markers
    for i := 1; i < rows; i++ {
        for j := 1; j < cols; j++ {
            if matrix[i][j] == 0 {
                matrix[i][0] = 0
                matrix[0][j] = 0
            }
        }
    }
    
    // Zero out cells based on markers
    for i := 1; i < rows; i++ {
        for j := 1; j < cols; j++ {
            if matrix[i][0] == 0 || matrix[0][j] == 0 {
                matrix[i][j] = 0
            }
        }
    }
    
    // Zero out the first column if needed
    if colZero {
        for i := 0; i < rows; i++ {
            matrix[i][0] = 0
        }
    }
    
    // Zero out the first row if needed
    if rowZero {
        for j := 0; j < cols; j++ {
            matrix[0][j] = 0
        }
    }
}
```

### Complexity
- **Time Complexity:** O(m * n), where m and n are the number of rows and columns, respectively.
- **Space Complexity:** O(1), utilizing existing matrix's first row and column for tracking.

