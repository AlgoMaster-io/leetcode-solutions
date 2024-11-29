# [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Approach 1: Simulation with Boundaries (Optimal)

### Solution
```go
// Time Complexity: O(m * n)
// Space Complexity: O(1) (excluding output list)
package main

func spiralOrder(matrix [][]int) []int {
    result := []int{}

    if matrix == nil || len(matrix) == 0 {
        return result
    }

    top, bottom := 0, len(matrix)-1
    left, right := 0, len(matrix[0])-1

    for top <= bottom && left <= right {
        // Traverse from left to right
        for i := left; i <= right; i++ {
            result = append(result, matrix[top][i])
        }
        top++

        // Traverse from top to bottom
        for i := top; i <= bottom; i++ {
            result = append(result, matrix[i][right])
        }
        right--

        // Traverse from right to left
        if top <= bottom {
            for i := right; i >= left; i-- {
                result = append(result, matrix[bottom][i])
            }
            bottom--
        }

        // Traverse from bottom to top
        if left <= right {
            for i := bottom; i >= top; i-- {
                result = append(result, matrix[i][left])
            }
            left++
        }
    }

    return result
}
```

## Approach 2: Simulated Layer Traversal (Alternative)

### Solution
```go
// Time Complexity: O(m * n)
// Space Complexity: O(1) (excluding output list)
package main

func spiralOrder(matrix [][]int) []int {
    result := []int{}

    if matrix == nil || len(matrix) == 0 {
        return result
    }

    rows, cols := len(matrix), len(matrix[0])
    startRow, startCol := 0, 0

    for startRow < rows && startCol < cols {
        // Traverse the top row
        for i := startCol; i < cols; i++ {
            result = append(result, matrix[startRow][i])
        }
        startRow++

        // Traverse the right column
        for i := startRow; i < rows; i++ {
            result = append(result, matrix[i][cols-1])
        }
        cols--

        // Traverse the bottom row
        if startRow < rows {
            for i := cols - 1; i >= startCol; i-- {
                result = append(result, matrix[rows-1][i])
            }
            rows--
        }

        // Traverse the left column
        if startCol < cols {
            for i := rows - 1; i >= startRow; i-- {
                result = append(result, matrix[i][startCol])
            }
            startCol++
        }
    }

    return result
}
```

