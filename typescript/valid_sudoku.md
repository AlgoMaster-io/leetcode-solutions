# 36. [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

## Approach 1: Using HashSets for Rows, Columns, and Sub-boxes

### Solution
typescript
```typescript
// Time Complexity: O(9^2) = O(81)
// Space Complexity: O(9 * 9 * 3) = O(243) ≈ O(1) since the board size is constant

function isValidSudoku(board: string[][]): boolean {
    const seen = new Set<string>();

    // Iterate through the board
    for (let i = 0; i < 9; i++) {
        for (let j = 0; j < 9; j++) {
            const num = board[i][j];

            // Ignore empty cells
            if (num !== '.') {
                // Construct unique keys for rows, columns, and boxes
                const rowKey = `row${i}${num}`;
                const colKey = `col${j}${num}`;
                const boxKey = `box${Math.floor(i / 3)}${Math.floor(j / 3)}${num}`;

                // Check if the keys already exist
                if (seen.has(rowKey) || seen.has(colKey) || seen.has(boxKey)) {
                    return false;
                }

                // Add keys to set
                seen.add(rowKey);
                seen.add(colKey);
                seen.add(boxKey);
            }
        }
    }

    return true;
}
```

## Approach 2: Using Arrays for Optimization

### Solution
typescript
```typescript
// Time Complexity: O(81) = O(1) for a fixed 9x9 board
// Space Complexity: O(81) = O(1)

function isValidSudoku(board: string[][]): boolean {
    const rows = Array.from({ length: 9 }, () => new Array(9).fill(false));
    const cols = Array.from({ length: 9 }, () => new Array(9).fill(false));
    const boxes = Array.from({ length: 9 }, () => new Array(9).fill(false));

    // Iterate through the board
    for (let i = 0; i < 9; i++) {
        for (let j = 0; j < 9; j++) {
            if (board[i][j] !== '.') {
                const num = board[i][j].charCodeAt(0) - '1'.charCodeAt(0); // Map '1'-'9' to 0-8
                const boxIndex = Math.floor(i / 3) * 3 + Math.floor(j / 3);

                // Check rows, columns, and boxes
                if (rows[i][num] || cols[j][num] || boxes[boxIndex][num]) {
                    return false;
                }

                // Mark the number as seen
                rows[i][num] = true;
                cols[j][num] = true;
                boxes[boxIndex][num] = true;
            }
        }
    }

    return true;
}
```

