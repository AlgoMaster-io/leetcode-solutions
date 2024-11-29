# [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

## Approach 1: Using Additional Arrays (Basic)

### Solution
go
```go
// Time Complexity: O(m * n)
// Space Complexity: O(m + n)
func setZeroes(matrix [][]int) {
    rows := len(matrix)
    cols := len(matrix[0])
    
    rowZero := make([]bool, rows)
    colZero := make([]bool, cols)

    // Record rows and columns that need to be zeroed
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if matrix[i][j] == 0 {
                rowZero[i] = true
                colZero[j] = true
            }
        }
    }

    // Set rows to zero
    for i := 0; i < rows; i++ {
        if rowZero[i] {
            for j := 0; j < cols; j++ {
                matrix[i][j] = 0
            }
        }
    }

    // Set columns to zero
    for j := 0; j < cols; j++ {
        if colZero[j] {
            for i := 0; i < rows; i++ {
                matrix[i][j] = 0
            }
        }
    }
}
```

## Approach 2: Using First Row and Column as Markers (Optimal)

### Solution
go
```go
// Time Complexity: O(m * n)
// Space Complexity: O(1)
func setZeroes(matrix [][]int) {
    rows := len(matrix)
    cols := len(matrix[0])
    firstRowZero := false
    firstColZero := false

    // Check if first row needs to be zeroed
    for j := 0; j < cols; j++ {
        if matrix[0][j] == 0 {
            firstRowZero = true
            break
        }
    }

    // Check if first column needs to be zeroed
    for i := 0; i < rows; i++ {
        if matrix[i][0] == 0 {
            firstColZero = true
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

    // Set elements to zero based on markers
    for i := 1; i < rows; i++ {
        for j := 1; j < cols; j++ {
            if matrix[i][0] == 0 || matrix[0][j] == 0 {
                matrix[i][j] = 0
            }
        }
    }

    // Zero out the first row if needed
    if firstRowZero {
        for j := 0; j < cols; j++ {
            matrix[0][j] = 0
        }
    }

    // Zero out the first column if needed
    if firstColZero {
        for i := 0; i < rows; i++ {
            matrix[i][0] = 0
        }
    }
}
```

