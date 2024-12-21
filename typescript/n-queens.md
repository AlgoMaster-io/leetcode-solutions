# [Leetcode 51: N-Queens](https://leetcode.com/problems/n-queens/)

## Approaches
- [Backtracking Approach](#backtracking-approach)

## Backtracking Approach

### Intuition
The N-Queens problem requires placing N queens on an N×N chessboard such that no two queens threaten each other. This means no two queens can be in the same row, column, or diagonal. The most intuitive way to solve this problem is using backtracking, a method to solve recursive problems by building a solution incrementally and removing solutions that fail to satisfy the constraints.

### Implementation
In this approach, we'll explore potential positions for placing queens row by row. For each row, we attempt to place a queen in each column, checking if the position is valid (i.e., not threatened by another queen). If valid, we proceed to place queens in the next row. If we can't place any queen in the current row, we backtrack to the previous row and try the next possible position.

### Code
```typescript
function solveNQueens(n: number): string[][] {
    const solutions: string[][] = [];
    const board: string[] = Array(n).fill('.'.repeat(n));

    const isValid = (row: number, col: number, board: string[]): boolean => {
        // Check column
        for (let i = 0; i < row; i++) {
            if (board[i][col] === 'Q') return false;
        }
        // Check diagonal (top-left to bottom-right)
        for (let i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] === 'Q') return false;
        }
        // Check anti-diagonal (top-right to bottom-left)
        for (let i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board[i][j] === 'Q') return false;
        }
        return true;
    };

    const backtrack = (row: number): void => {
        if (row === n) {
            solutions.push([...board]);  // all queens placed
            return;
        }

        for (let col = 0; col < n; col++) {
            if (isValid(row, col, board)) {
                // Place the queen
                board[row] = board[row].substr(0, col) + 'Q' + board[row].substr(col + 1);
                backtrack(row + 1);
                // Remove the queen (backtrack)
                board[row] = board[row].substr(0, col) + '.' + board[row].substr(col + 1);
            }
        }
    };

    backtrack(0);
    return solutions;
}
```

### Detailed Comments:
- **isValid function**: Checks if placing a queen at `row` and `col` is safe. We only check rows and diagonals above the current `row` because queens are placed row by row.
- **backtrack function**: Tries to place a queen in each column of the current row. If succeeded, move on to the next row, otherwise backtrack by removing the queen and trying the next column.
- **Base Case**: When `row === n`, it means all queens have been placed, and we add the current board configuration to our solutions.

### Time and Space Complexity
- **Time Complexity**: O(N!), The problem roughly branches N possibilities at each step but is reduced due to pruning with invalid positions.
- **Space Complexity**: O(N^2), for storing the board and solutions, explicitly and implicitly due to recursion call stack.

