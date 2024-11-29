# 51. [N-Queens](https://leetcode.com/problems/n-queens/)

## Approach 1: Backtracking

### Solution
```javascript
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n^2) for storing the board state

var solveNQueens = function(n) {
    const result = [];
    const board = Array.from({ length: n }, () => Array(n).fill('.'));

    const backtrack = function(row) {
        if (row === n) {
            result.push(construct(board));
            return;
        }

        for (let col = 0; col < n; col++) {
            if (isSafe(board, row, col)) {
                board[row][col] = 'Q'; // Place the queen
                backtrack(row + 1); // Recurse to the next row
                board[row][col] = '.'; // Backtrack and remove the queen
            }
        }
    }

    const isSafe = function(board, row, col) {
        // Check the column
        for (let i = 0; i < row; i++) {
            if (board[i][col] === 'Q') {
                return false;
            }
        }

        // Check the diagonal (top-left to bottom-right)
        for (let i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] === 'Q') {
                return false;
            }
        }

        // Check the anti-diagonal (top-right to bottom-left)
        for (let i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board[i][j] === 'Q') {
                return false;
            }
        }

        return true; // The position is safe for a queen
    }

    const construct = function(board) {
        return board.map(row => row.join(''));
    }

    backtrack(0);
    return result;
}
```

## Approach 2: Optimized Backtracking with Bitmasking

### Solution
```javascript
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n) for storing the column, diagonal, and anti-diagonal states

var solveNQueens = function(n) {
    const result = [];
    const queens = new Array(n).fill(-1);

    const backtrack = function(row) {
        if (row === n) {
            result.push(construct(queens, n));
            return;
        }

        for (let col = 0; col < n; col++) {
            if (isSafe(row, col, queens)) {
                queens[row] = col; // Place the queen in the current row
                backtrack(row + 1); // Recurse to the next row
            }
        }
    }

    const isSafe = function(row, col, queens) {
        for (let i = 0; i < row; i++) {
            if (queens[i] === col || Math.abs(queens[i] - col) === Math.abs(i - row)) {
                return false; // Check for column and diagonal conflicts
            }
        }
        return true;
    }

    const construct = function(queens, n) {
        const board = [];
        for (let i = 0; i < n; i++) {
            let row = Array(n).fill('.');
            row[queens[i]] = 'Q';
            board.push(row.join(''));
        }
        return board;
    }

    backtrack(0);
    return result;
}
```

