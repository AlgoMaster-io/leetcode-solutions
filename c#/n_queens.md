# 51. [N-Queens](https://leetcode.com/problems/n-queens/)

## Approach 1: Backtracking

### Solution
csharp
```csharp
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n^2) for storing the board state
using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<string>> SolveNQueens(int n) {
        IList<IList<string>> result = new List<IList<string>>();
        char[,] board = new char[n, n];

        // Initialize the board with empty cells
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                board[i, j] = '.';
            }
        }
        
        Backtrack(board, 0, result);
        return result;
    }

    private void Backtrack(char[,] board, int row, IList<IList<string>> result) {
        if (row == board.GetLength(0)) {
            result.Add(Construct(board)); // Add the current board configuration to the result
            return;
        }

        for (int col = 0; col < board.GetLength(1); col++) {
            if (IsSafe(board, row, col)) {
                board[row, col] = 'Q'; // Place the queen
                Backtrack(board, row + 1, result); // Recurse to the next row
                board[row, col] = '.'; // Backtrack and remove the queen
            }
        }
    }

    private bool IsSafe(char[,] board, int row, int col) {
        // Check the column
        for (int i = 0; i < row; i++) {
            if (board[i, col] == 'Q') {
                return false;
            }
        }

        // Check the diagonal (top-left to bottom-right)
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i, j] == 'Q') {
                return false;
            }
        }

        // Check the anti-diagonal (top-right to bottom-left)
        for (int i = row - 1, j = col + 1; i >= 0 && j < board.GetLength(1); i--, j++) {
            if (board[i, j] == 'Q') {
                return false;
            }
        }

        return true; // The position is safe for a queen
    }

    private IList<string> Construct(char[,] board) {
        List<string> current = new List<string>();
        for (int i = 0; i < board.GetLength(0); i++) {
            char[] row = new char[board.GetLength(1)];
            for (int j = 0; j < board.GetLength(1); j++) {
                row[j] = board[i, j];
            }
            current.Add(new string(row));
        }
        return current;
    }
}
```

## Approach 2: Optimized Backtracking with Bitmasking

### Solution
csharp
```csharp
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n) for storing the column, diagonal, and anti-diagonal states
using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<string>> SolveNQueens(int n) {
        IList<IList<string>> result = new List<IList<string>>();
        Backtrack(0, n, new int[n], result);
        return result;
    }

    private void Backtrack(int row, int n, int[] queens, IList<IList<string>> result) {
        if (row == n) {
            result.Add(Construct(queens, n)); // Add the current board configuration to the result
            return;
        }

        for (int col = 0; col < n; col++) {
            if (IsSafe(row, col, queens)) {
                queens[row] = col; // Place the queen in the current row
                Backtrack(row + 1, n, queens, result); // Recurse to the next row
            }
        }
    }

    private bool IsSafe(int row, int col, int[] queens) {
        for (int i = 0; i < row; i++) {
            if (queens[i] == col || Math.Abs(queens[i] - col) == Math.Abs(i - row)) {
                return false; // Check for column and diagonal conflicts
            }
        }
        return true;
    }

    private IList<string> Construct(int[] queens, int n) {
        IList<string> board = new List<string>();
        for (int i = 0; i < n; i++) {
            char[] row = new char[n];
            for (int j = 0; j < n; j++) {
                row[j] = (queens[i] == j) ? 'Q' : '.';
            }
            board.Add(new string(row));
        }
        return board;
    }
}
```

