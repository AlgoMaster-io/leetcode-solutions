# 51. [N-Queens](https://leetcode.com/problems/n-queens/)

## Approach 1: Backtracking

### Solution
python
```python
# Time Complexity: O(n!), where n is the size of the board
# Space Complexity: O(n^2) for storing the board state

class Solution:
    def solveNQueens(self, n):
        def isSafe(board, row, col):
            # Check the column
            for i in range(row):
                if board[i][col] == 'Q':
                    return False

            # Check the diagonal (top-left to bottom-right)
            i, j = row - 1, col - 1
            while i >= 0 and j >= 0:
                if board[i][j] == 'Q':
                    return False
                i -= 1
                j -= 1

            # Check the anti-diagonal (top-right to bottom-left)
            i, j = row - 1, col + 1
            while i >= 0 and j < n:
                if board[i][j] == 'Q':
                    return False
                i -= 1
                j += 1

            return True

        def construct(board):
            current = []
            for i in range(n):
                current.append("".join(board[i]))
            return current

        def backtrack(board, row, result):
            if row == n:
                result.append(construct(board))
                return

            for col in range(n):
                if isSafe(board, row, col):
                    board[row][col] = 'Q'
                    backtrack(board, row + 1, result)
                    board[row][col] = '.'

        result = []
        board = [['.'] * n for _ in range(n)]
        backtrack(board, 0, result)
        return result
```

## Approach 2: Optimized Backtracking with Bitmasking

### Solution
python
```python
# Time Complexity: O(n!), where n is the size of the board
# Space Complexity: O(n) for storing the column, diagonal, and anti-diagonal states

class Solution:
    def solveNQueens(self, n):
        def isSafe(row, col, queens):
            for i in range(row):
                if queens[i] == col or abs(queens[i] - col) == abs(i - row):
                    return False
            return True

        def construct(queens, n):
            board = []
            for i in range(n):
                row = ['.'] * n
                row[queens[i]] = 'Q'
                board.append("".join(row))
            return board

        def backtrack(row, n, queens, result):
            if row == n:
                result.append(construct(queens, n))
                return

            for col in range(n):
                if isSafe(row, col, queens):
                    queens[row] = col
                    backtrack(row + 1, n, queens, result)

        result = []
        queens = [-1] * n
        backtrack(0, n, queens, result)
        return result
```


