# [Problem 36: Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

In this problem, you are asked to determine if a 9x9 Sudoku board is valid. According to the rules of Sudoku, each row, each column, and each of the nine 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

## Approaches:

1. [Naive Approach: Using Arrays and Iterative Validation](#naive-approach)
2. [Optimized Approach: Using Sets for Simultaneous Validation](#optimized-approach)

---

## Naive Approach

The simplest way to solve the problem is to check each row, column, and 3x3 sub-box individually to ensure that there are no duplicate numbers.

### Intuition
- We iterate over each element of the board.
- For each element, we check:
  1. If the number is already present in the current row.
  2. If the number is already present in the current column.
  3. If the number is already present in the current 3x3 sub-box.
- If any of these checks fail (i.e., a duplicate is found), the board is invalid.

### Time Complexity
- **O(N^2)**, where N is the size of the board (9 for a Sudoku board). We iterate over each element and perform constant time operations.

### Space Complexity
- **O(N)**, for the arrays used to store row, column, and sub-box data.

### Solution

```go
func isValidSudoku(board [][]byte) bool {
    // Initialize structures to hold counts of seen numbers per row, column and sub-box.
    rows := make([][]bool, 9)
    columns := make([][]bool, 9)
    boxes := make([][]bool, 9)

    for i := 0; i < 9; i++ {
        rows[i] = make([]bool, 9)
        columns[i] = make([]bool, 9)
        boxes[i] = make([]bool, 9)
    }

    // Iterate over each cell in the board.
    for i := 0; i < 9; i++ {
        for j := 0; j < 9; j++ {
            if board[i][j] == '.' {
                continue // Ignore empty cells.
            }
            
            num := board[i][j] - '1' // Map '1' -> 0, ..., '9' -> 8

            // Check the row.
            if rows[i][num] {
                return false // Duplicate found in row
            }
            rows[i][num] = true
            
            // Check the column.
            if columns[j][num] {
                return false // Duplicate found in column
            }
            columns[j][num] = true
            
            // Determine the box index.
            boxIndex := (i/3) * 3 + j/3
            if boxes[boxIndex][num] {
                return false // Duplicate found in box
            }
            boxes[boxIndex][num] = true
        }
    }

    return true
}
```

---

## Optimized Approach

You can optimize the solution by combining the checks for rows, columns, and boxes into a single pass through the board using hash sets to track seen numbers.

### Intuition
- We create hash maps (or sets) to track numbers in rows, columns, and boxes.
- As we iterate through the board, we immediately validate each number and update these structures.
- This allows us to simultaneously validate rows, columns, and boxes in a single pass.

### Time Complexity
- **O(N^2)**: We still iterate over each element once but use more optimal data structures for lookups.

### Space Complexity
- **O(N)** for each hash set.

### Solution

```go
func isValidSudokuOptimized(board [][]byte) bool {
    rows := make([]map[byte]bool, 9)
    columns := make([]map[byte]bool, 9)
    boxes := make([]map[byte]bool, 9)

    for i := 0; i < 9; i++ {
        rows[i] = make(map[byte]bool)
        columns[i] = make(map[byte]bool)
        boxes[i] = make(map[byte]bool)
    }

    // Iterate over each cell in the Sudoku board.
    for i := 0; i < 9; i++ {
        for j := 0; j < 9; j++ {
            num := board[i][j]
            if num == '.' {
                continue // Ignore empty cells.
            }

            // Determine which sub-box we're in.
            boxIndex := (i/3)*3 + j/3

            // Check if the number is already seen in the row, column, or box.
            if rows[i][num] || columns[j][num] || boxes[boxIndex][num] {
                return false // Found a duplicate
            }

            // Mark the number as seen in the corresponding row, column, and box.
            rows[i][num] = true
            columns[j][num] = true
            boxes[boxIndex][num] = true
        }
    }

    return true
}
```

---

This problem demonstrates how to validate matrices systematically by controlling iterations and using auxiliary storage. The transition from naive to optimized approach is a good exercise in managing data structures to reduce complexity.

