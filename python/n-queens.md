# [Leetcode 51: N-Queens](https://leetcode.com/problems/n-queens/)

## Approaches
1. [Backtracking with Board Setup](#backtracking-with-board-setup)
2. [Optimized Backtracking with Boolean Arrays](#optimized-backtracking-with-boolean-arrays)
3. [Most Optimal: Backtracking with Bit Manipulation](#most-optimal-backtracking-with-bit-manipulation)

---

## 1. Backtracking with Board Setup

### Intuition
The n-queens problem is a classic backtracking problem where we are asked to place n queens on an n×n chessboard such that no two queens threaten each other. The fundamental approach involves placing queens on the board row by row and using recursion to explore all possible combinations. If a position is valid, place the queen and proceed to the next row. If not, backtrack and try the next position.

### Python Code
```python
def solveNQueens(n):
    def is_valid(board, row, col):
        # Check if there's a queen in the same column
        for i in range(row):
            if board[i][col] == 'Q':
                return False
        # Check the upper left diagonal
        i, j = row, col
        while i >= 0 and j >= 0:
            if board[i][j] == 'Q':
                return False
            i -= 1
            j -= 1
        # Check the upper right diagonal
        i, j = row, col
        while i >= 0 and j < n:
            if board[i][j] == 'Q':
                return False
            i -= 1
            j += 1
        return True

    def backtrack(row, board):
        if row == n:
            # If all rows are filled successfully, add to result
            solution = [''.join(r) for r in board]
            results.append(solution)
            return
        for col in range(n):
            if is_valid(board, row, col):
                board[row][col] = 'Q'
                backtrack(row + 1, board)
                # Backtrack
                board[row][col] = '.'

    results = []
    board = [['.' for _ in range(n)] for _ in range(n)]
    backtrack(0, board)
    return results

# Example usage
print(solveNQueens(4))
```

### Time Complexity
- O(N!) for placing N queens, as the solution checks all possible permutations.
  
### Space Complexity
- O(N^2) to store the board.

---

## 2. Optimized Backtracking with Boolean Arrays

### Intuition
Instead of using a board with 'Q' and '.', use boolean arrays to track column, main diagonal, and secondary diagonal attacks. This reduces complexity and avoids repeatedly checking the board for conflicts, speeding up the algorithm by precluding exploration of invalid states. 

### Python Code
```python
def solveNQueens(n):
    def backtrack(row):
        if row == n:
            solutions.append([''.join(r) for r in board])
            return
        for col in range(n):
            if not cols[col] and not main_diag[row - col] and not sec_diag[row + col]:
                # Place queen
                cols[col] = main_diag[row - col] = sec_diag[row + col] = True
                board[row][col] = 'Q'
                backtrack(row + 1)
                # Remove queen (Backtrack)
                cols[col] = main_diag[row - col] = sec_diag[row + col] = False
                board[row][col] = '.'

    solutions = []
    cols = [False] * n
    main_diag = [False] * (2 * n - 1)
    sec_diag = [False] * (2 * n - 1)
    board = [['.'] * n for _ in range(n)]
    backtrack(0)
    return solutions

# Example usage
print(solveNQueens(4))
```
  
### Time Complexity
- O(N!) as we still explore permutations but reduce redundant operations.

### Space Complexity
- O(N) for tracking columns and diagonals.

---

## 3. Most Optimal: Backtracking with Bit Manipulation

### Intuition
Bit manipulation provides a very compact and efficient way to track states. Use bits to represent availability of columns and diagonals. This approach effectively reduces computational overhead by packing state information into a few integers, leveraging the efficiency of bitwise operations.

### Python Code
```python
def solveNQueens(n):
    def backtrack(row, cols, d1, d2):
        if row == n:
            solutions.append([''.join(r) for r in board])
            return
        # Get available positions
        available_positions = ~ (cols | d1 | d2) & ((1 << n) - 1)
        while available_positions:
            # Isolate the rightmost 1-bit.
            position = available_positions & -available_positions
            # position indicates where we can place the queen
            available_positions &= available_positions - 1
            col = (position - 1).bit_length()
            board[row][col] = 'Q'
            # Recurse for the next row
            backtrack(row + 1, cols | position, (d1 | position) << 1, (d2 | position) >> 1)
            board[row][col] = '.'

    solutions = []
    board = [['.'] * n for _ in range(n)]
    backtrack(0, 0, 0, 0)
    return solutions

# Example usage
print(solveNQueens(4))
```

### Time Complexity
- O(N!) because it explores factorically reduced search space with optimal updates.

### Space Complexity
- O(N) again for storing the recursion stack and the solution state with arrays for the board representation. 

This approach substantially increases the performance for larger n due to the bitwise operations cutting down redundant calculations made in each function call.

