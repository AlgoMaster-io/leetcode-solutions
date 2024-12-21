# [N-Queens](https://leetcode.com/problems/n-queens/)

## Approaches
1. [Basic Backtracking Solution](#basic-backtracking-solution)
2. [Optimized Backtracking with Bitmasking](#optimized-backtracking-with-bitmasking)

### Basic Backtracking Solution

The N-Queens problem is about placing N queens on an N×N chessboard such that no two queens threaten each other. The simplest way to approach this problem is through backtracking. The idea is to place queens one by one in different columns, starting from the leftmost column. When placing a queen in a column, we check if it is safe: can't be attacked by any previously placed queens.

**Intuition:**
- Place a queen in an empty cell in the current column.
- If the placement is safe from all previously placed queens, move to the next column and try to place a queen there.
- If placing a queen in the current column doesn't allow you to place all N queens, backtrack: remove the queen (this cell is not viable) and try other cells in the current column.
- Continue this process until all columns are processed.

```go
package main

import (
	"fmt"
	"strings"
)

func solveNQueens(n int) [][]string {
    var result [][]string
    board := make([][]string, n)
    for i := range board {
        board[i] = make([]string, n)
        for j := range board[i] {
            board[i][j] = "."
        }
    }
    solve(0, board, &result, n)
    return result
}

func solve(col int, board [][]string, result *[][]string, n int) {
    if col == n {
        solution := []string{}
        for _, row := range board {
            solution = append(solution, strings.Join(row, ""))
        }
        *result = append(*result, solution)
        return
    }
    
    for row := 0; row < n; row++ {
        if isSafe(row, col, board, n) {
            board[row][col] = "Q" // place the queen
            solve(col+1, board, result, n) // go for the next column
            board[row][col] = "." // backtrack
        }
    }
}

// Function to check if a queen can be placed at board[row][col]
func isSafe(row, col int, board [][]string, n int) bool {
    // Check left side of row
    for i := 0; i < col; i++ {
        if board[row][i] == "Q" {
            return false
        }
    }

    // Check upper diagonal on left side
    for i, j := row, col; i >= 0 && j >= 0; i, j = i-1, j-1 {
        if board[i][j] == "Q" {
            return false
        }
    }

    // Check lower diagonal on left side
    for i, j := row, col; j >= 0 && i < n; i, j = i+1, j-1 {
        if board[i][j] == "Q" {
            return false
        }
    }

    return true
}
```

**Time Complexity:** O(N!)

**Space Complexity:** O(N^2) for storing the board.

### Optimized Backtracking with Bitmasking

In this approach, we can reduce the space complexity by using bitmasking. Instead of storing the entire board, we use three bitmasks to keep track of which columns, and diagonals are occupied by queens.
- `columns`: keeps track of which columns have queens.
- `diag1`: keeps track of the "main digonal" (\- direction), identified by `row - col`.
- `diag2`: keeps track of the "anti-diagonal" (/ direction), identified by `row + col`.

**Intuition:**
- Similar backtracking logic as before, but now we efficiently check for safe placement using bitmask operations.
- This approach reduces additional space, as we do not store the board.

```go
package main

import (
	"fmt"
	"strings"
)

func solveNQueens(n int) [][]string {
	var result [][]string
	var board []string = make([]string, n)
	for i := range board {
		board[i] = strings.Repeat(".", n)
	}
	solve(0, n, 0, 0, 0, &board, &result)
	return result
}

func solve(row, n, columns, diag1, diag2 int, board *[]string, result *[][]string) {
	if row == n {
		solution := make([]string, len(*board))
		copy(solution, *board)
		*result = append(*result, solution)
		return
	}

	for col := 0; col < n; col++ {
        // Calculate the positions for diagonals
		currDiagonals1 := row - col + n - 1
		currDiagonals2 := row + col

		if columns&(1<<col) == 0 && diag1&(1<<currDiagonals1) == 0 && diag2&(1<<currDiagonals2) == 0 {
			// Place the queen
			(*board)[row] = (*board)[row][:col] + "Q" + (*board)[row][col+1:]
			
			// Mark the attacks by setting the bits
			solve(row+1, n, columns|(1<<col), diag1|(1<<currDiagonals1), diag2|(1<<currDiagonals2), board, result)
			
			// Remove the queen - this helps to backtrack
			(*board)[row] = (*board)[row][:col] + "." + (*board)[row][col+1:]
		}
	}
}
```

**Time Complexity:** O(N!)

**Space Complexity:** O(N) for the bitmasks, which is significantly less than maintaining an N×N board.

By efficiently using backtracking and bitmasking, the N-Queens problem can be solved with reduced space complexity while maintaining the original intuition and solution structure.

