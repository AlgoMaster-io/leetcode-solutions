# [36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

## Approach 1: Using HashSets for Rows, Columns, and Sub-boxes

### Solution
go
```go
// Time Complexity: O(9^2) = O(81)
// Space Complexity: O(9 * 9 * 3) = O(243) ≈ O(1) since the board size is constant
package main

import (
	"fmt"
)

func isValidSudoku(board [][]byte) bool {
	seen := make(map[string]bool)

	// Iterate through the board
	for i := 0; i < 9; i++ {
		for j := 0; j < 9; j++ {
			num := board[i][j]

			// Ignore empty cells
			if num != '.' {
				// Construct unique keys for rows, columns, and boxes
				rowKey := fmt.Sprintf("row%d%c", i, num)
				colKey := fmt.Sprintf("col%d%c", j, num)
				boxKey := fmt.Sprintf("box%d%d%c", i/3, j/3, num)

				// Check if the keys already exist
				if seen[rowKey] || seen[colKey] || seen[boxKey] {
					return false
				}

				// Mark as seen
				seen[rowKey] = true
				seen[colKey] = true
				seen[boxKey] = true
			}
		}
	}

	return true
}
```

## Approach 2: Using Arrays for Optimization

### Solution
go
```go
// Time Complexity: O(81) = O(1) for a fixed 9x9 board
// Space Complexity: O(81) = O(1)
package main

func isValidSudoku(board [][]byte) bool {
	rows := [9][9]bool{}
	cols := [9][9]bool{}
	boxes := [9][9]bool{}

	// Iterate through the board
	for i := 0; i < 9; i++ {
		for j := 0; j < 9; j++ {
			if board[i][j] != '.' {
				num := board[i][j] - '1' // Map '1'-'9' to 0-8
				boxIndex := (i/3)*3 + j/3

				// Check rows, columns, and boxes
				if rows[i][num] || cols[j][num] || boxes[boxIndex][num] {
					return false
				}

				// Mark the number as seen
				rows[i][num] = true
				cols[j][num] = true
				boxes[boxIndex][num] = true
			}
		}
	}

	return true
}
```


