# 51. [N-Queens](https://leetcode.com/problems/n-queens/)

## Approach 1: Backtracking

### Solution
```java
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n^2) for storing the board state
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> result = new ArrayList<>();
        char[][] board = new char[n][n];

        // Initialize the board with empty cells
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = '.';
            }
        }

        backtrack(board, 0, result);
        return result;
    }

    private void backtrack(char[][] board, int row, List<List<String>> result) {
        if (row == board.length) {
            result.add(construct(board)); // Add the current board configuration to the result
            return;
        }

        for (int col = 0; col < board.length; col++) {
            if (isSafe(board, row, col)) {
                board[row][col] = 'Q'; // Place the queen
                backtrack(board, row + 1, result); // Recurse to the next row
                board[row][col] = '.'; // Backtrack and remove the queen
            }
        }
    }

    private boolean isSafe(char[][] board, int row, int col) {
        // Check the column
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q') {
                return false;
            }
        }

        // Check the diagonal (top-left to bottom-right)
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }

        // Check the anti-diagonal (top-right to bottom-left)
        for (int i = row - 1, j = col + 1; i >= 0 && j < board.length; i--, j++) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }

        return true; // The position is safe for a queen
    }

    private List<String> construct(char[][] board) {
        List<String> current = new ArrayList<>();
        for (int i = 0; i < board.length; i++) {
            current.add(new String(board[i]));
        }
        return current;
    }
}
```

## Approach 2: Optimized Backtracking with Bitmasking

### Solution
```java
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n) for storing the column, diagonal, and anti-diagonal states
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> result = new ArrayList<>();
        backtrack(0, n, new int[n], result);
        return result;
    }

    private void backtrack(int row, int n, int[] queens, List<List<String>> result) {
        if (row == n) {
            result.add(construct(queens, n)); // Add the current board configuration to the result
            return;
        }

        for (int col = 0; col < n; col++) {
            if (isSafe(row, col, queens)) {
                queens[row] = col; // Place the queen in the current row
                backtrack(row + 1, n, queens, result); // Recurse to the next row
            }
        }
    }

    private boolean isSafe(int row, int col, int[] queens) {
        for (int i = 0; i < row; i++) {
            if (queens[i] == col || Math.abs(queens[i] - col) == Math.abs(i - row)) {
                return false; // Check for column and diagonal conflicts
            }
        }
        return true;
    }

    private List<String> construct(int[] queens, int n) {
        List<String> board = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            char[] row = new char[n];
            for (int j = 0; j < n; j++) {
                row[j] = (queens[i] == j) ? 'Q' : '.';
            }
            board.add(new String(row));
        }
        return board;
    }
}
```