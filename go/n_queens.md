# 51. [N-Queens](https://leetcode.com/problems/n-queens/)

## Approach 1: Backtracking

### Solution
go
```go
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n^2) for storing the board state
package main

import "strings"

func solveNQueens(n int) [][]string {
	result := [][]string{}
	board := make([][]rune, n)

	// Initialize the board with empty cells
	for i := range board {
		board[i] = make([]rune, n)
		for j := range board[i] {
			board[i][j] = '.'
		}
	}

	backtrack(board, 0, &result)
	return result
}

func backtrack(board [][]rune, row int, result *[][]string) {
	if row == len(board) {
		*result = append(*result, construct(board)) // Add the current board configuration to the result
		return
	}

	for col := 0; col < len(board); col++ {
		if isSafe(board, row, col) {
			board[row][col] = 'Q' // Place the queen
			backtrack(board, row+1, result) // Recurse to the next row
			board[row][col] = '.' // Backtrack and remove the queen
		}
	}
}

func isSafe(board [][]rune, row, col int) bool {
	// Check the column
	for i := 0; i < row; i++ {
		if board[i][col] == 'Q' {
			return false
		}
	}

	// Check the diagonal (top-left to bottom-right)
	for i, j := row-1, col-1; i >= 0 && j >= 0; i, j = i-1, j-1 {
		if board[i][j] == 'Q' {
			return false
		}
	}

	// Check the anti-diagonal (top-right to bottom-left)
	for i, j := row-1, col+1; i >= 0 && j < len(board); i, j = i-1, j+1 {
		if board[i][j] == 'Q' {
			return false
		}
	}

	return true // The position is safe for a queen
}

func construct(board [][]rune) []string {
	current := []string{}
	for _, row := range board {
		current = append(current, string(row))
	}
	return current
}
```

## Approach 2: Optimized Backtracking with Bitmasking

### Solution
go
```go
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n) for storing the column, diagonal, and anti-diagonal states
package main

import (
	"strings"
)

func solveNQueens(n int) [][]string {
	result := [][]string{}
	queens := make([]int, n)
	backtrack(0, n, queens, &result)
	return result
}

func backtrack(row, n int, queens []int, result *[][]string) {
	if row == n {
		*result = append(*result, construct(queens, n)) // Add the current board configuration to the result
		return
	}

	for col := 0; col < n; col++ {
		if isSafe(row, col, queens) {
			queens[row] = col // Place the queen in the current row
			backtrack(row+1, n, queens, result) // Recurse to the next row
		}
	}
}

func isSafe(row, col int, queens []int) bool {
	for i := 0; i < row; i++ {
		if queens[i] == col || abs(queens[i]-col) == abs(i-row) {
			return false // Check for column and diagonal conflicts
		}
	}
	return true
}

func construct(queens []int, n int) []string {
	board := []string{}
	for i := 0; i < n; i++ {
		row := strings.Repeat(".", n)
		board = append(board, row[:queens[i]]+"Q"+row[queens[i]+1:])
	}
	return board
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

