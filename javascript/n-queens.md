# [Leetcode 51: N-Queens](https://leetcode.com/problems/n-queens/)

## Approaches
1. [Backtracking Algorithm - Basic Solution](#backtracking-algorithm---basic-solution)
2. [Backtracking with Optimized Conflict Detection](#backtracking-with-optimized-conflict-detection)

### Backtracking Algorithm - Basic Solution

**Intuition:**
The N-Queens problem is a classic combinatorial problem that asks how to place N queens on an N×N chessboard so that no two queens threaten each other. A queen can attack any other piece in the same row, column, or diagonal.

The backtracking approach involves placing a queen row by row and exploring valid configurations recursively. If a placement leads to an invalid configuration, we backtrack and try another position.

**Code:**
```javascript
function solveNQueens(n) {
    const results = [];
    const board = Array.from({ length: n }, () => Array(n).fill('.'));

    function isValid(row, col) {
        // Check the column
        for (let i = 0; i < row; i++) {
            if (board[i][col] === 'Q') return false;
        }

        // Check the left diagonal
        for (let i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] === 'Q') return false;
        }

        // Check the right diagonal
        for (let i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board[i][j] === 'Q') return false;
        }

        return true;
    }

    function backtrack(row) {
        if (row === n) {
            const solution = board.map(r => r.join(''));
            results.push(solution);
            return;
        }

        for (let col = 0; col < n; col++) {
            if (isValid(row, col)) {
                board[row][col] = 'Q';
                backtrack(row + 1); // Move to the next row
                board[row][col] = '.'; // Backtrack
            }
        }
    }

    backtrack(0);
    return results;
}

```

**Time Complexity:** O(N!), since for each row, we try to place the queen in N columns, and choice constraints push down to further rows.

**Space Complexity:** O(N^2) due to the board array storage and recursive call stack at most N deep.

### Backtracking with Optimized Conflict Detection

**Intuition:**
Improvements can be made by using additional arrays to keep track of column and diagonal conflicts, which streamlines the process of conflict detection and reduces computation overhead.

**Code:**
```javascript
function solveNQueensOptimized(n) {
    const results = [];
    const board = Array.from({ length: n }, () => Array(n).fill('.'));
    const columns = new Array(n).fill(false);
    const diagonals1 = new Array(2 * n - 1).fill(false); // "/"
    const diagonals2 = new Array(2 * n - 1).fill(false); // "\"

    function backtrack(row) {
        if (row === n) {
            const solution = board.map(r => r.join(''));
            results.push(solution);
            return;
        }

        for (let col = 0; col < n; col++) {
            if (!columns[col] && !diagonals1[row + col] && !diagonals2[row - col + n - 1]) {
                board[row][col] = 'Q';
                columns[col] = diagonals1[row + col] = diagonals2[row - col + n - 1] = true;
                backtrack(row + 1);
                board[row][col] = '.'; // Backtrack
                columns[col] = diagonals1[row + col] = diagonals2[row - col + n - 1] = false;
            }
        }
    }

    backtrack(0);
    return results;
}
```

**Time Complexity:** O(N!) - Still primarily bounded by the combinatorial nature of placements.

**Space Complexity:** O(N) due to the additional tracking arrays for columns and diagonals, leads to improvement overall in terms of practical execution time.

