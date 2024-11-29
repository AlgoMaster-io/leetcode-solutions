# [48. Rotate Image](https://leetcode.com/problems/rotate-image/)

## Approach 1: Transpose and Reverse Rows (Optimal)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

func rotate(matrix [][]int) {
    n := len(matrix)

    // Step 1: Transpose the matrix
    for i := 0; i < n; i++ {
        for j := i; j < n; j++ {
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
        }
    }

    // Step 2: Reverse each row
    for i := 0; i < n; i++ {
        for j := 0; j < n/2; j++ {
            matrix[i][j], matrix[i][n-j-1] = matrix[i][n-j-1], matrix[i][j]
        }
    }
}
```

## Approach 2: Rotating Layer by Layer (In-Place)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

func rotate(matrix [][]int) {
    n := len(matrix)

    // Rotate each layer
    for layer := 0; layer < n/2; layer++ {
        first := layer
        last := n - layer - 1

        for i := first; i < last; i++ {
            offset := i - first

            // Save top element
            top := matrix[first][i]

            // Move left to top
            matrix[first][i] = matrix[last-offset][first]

            // Move bottom to left
            matrix[last-offset][first] = matrix[last][last-offset]

            // Move right to bottom
            matrix[last][last-offset] = matrix[i][last]

            // Move top to right
            matrix[i][last] = top
        }
    }
}
```

