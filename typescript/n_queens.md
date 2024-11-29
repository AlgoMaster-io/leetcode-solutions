# 51. [N-Queens](https://leetcode.com/problems/n-queens/)

## Approach 1: Backtracking

### Solution
typescript
```typescript
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n^2) for storing the board state

function solveNQueens(n: number): string[][] {
    const result: string[][] = [];
    const board: string[][] = Array.from({ length: n }, () => Array(n).fill('.'));

    const backtrack = (row: number) => {
        if (row === n) {
            result.push(construct(board));
            return;
        }

        for (let col = 0; col < n; col++) {
            if (isSafe(board, row, col)) {
                board[row][col] = 'Q';
                backtrack(row + 1);
                board[row][col] = '.';
            }
        }
    };

    const isSafe = (board: string[][], row: number, col: number): boolean => {
        for (let i = 0; i < row; i++) {
            if (board[i][col] === 'Q') return false;
        }

        for (let i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] === 'Q') return false;
        }

        for (let i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board[i][j] === 'Q') return false;
        }

        return true;
    };

    const construct = (board: string[][]): string[] => {
        return board.map(row => row.join(''));
    };

    backtrack(0);
    return result;
}
```

## Approach 2: Optimized Backtracking with Bitmasking

### Solution
typescript
```typescript
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n) for storing the column, diagonal, and anti-diagonal states

function solveNQueens(n: number): string[][] {
    const result: string[][] = [];
    const queens: number[] = new Array(n).fill(-1);

    const backtrack = (row: number): void => {
        if (row === n) {
            result.push(construct(queens, n));
            return;
        }

        for (let col = 0; col < n; col++) {
            if (isSafe(row, col, queens)) {
                queens[row] = col;
                backtrack(row + 1);
            }
        }
    };

    const isSafe = (row: number, col: number, queens: number[]): boolean => {
        for (let i = 0; i < row; i++) {
            if (queens[i] === col || Math.abs(queens[i] - col) === Math.abs(i - row)) {
                return false;
            }
        }
        return true;
    };

    const construct = (queens: number[], n: number): string[] => {
        return queens.map(col => {
            const row = Array(n).fill('.');
            row[col] = 'Q';
            return row.join('');
        });
    };

    backtrack(0);
    return result;
}
```

