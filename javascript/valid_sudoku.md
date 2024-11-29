# 36. [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

## Approach 1: Using HashSets for Rows, Columns, and Sub-boxes

### Solution
```javascript
// Time Complexity: O(9^2) = O(81)
// Space Complexity: O(9 * 9 * 3) = O(243) ≈ O(1) since the board size is constant

/**
 * @param {character[][]} board
 * @return {boolean}
 */
var isValidSudoku = function(board) {
    let seen = new Set();

    // Iterate through the board
    for (let i = 0; i < 9; i++) {
        for (let j = 0; j < 9; j++) {
            let num = board[i][j];

            // Ignore empty cells
            if (num !== '.') {
                // Construct unique keys for rows, columns, and boxes
                let rowKey = `row${i}${num}`;
                let colKey = `col${j}${num}`;
                let boxKey = `box${Math.floor(i / 3)}${Math.floor(j / 3)}${num}`;

                // Check if the keys already exist
                if (seen.has(rowKey) || seen.has(colKey) || seen.has(boxKey)) {
                    return false;
                }
                seen.add(rowKey);
                seen.add(colKey);
                seen.add(boxKey);
            }
        }
    }

    return true;
};
```

## Approach 2: Using Arrays for Optimization

### Solution
```javascript
// Time Complexity: O(81) = O(1) for a fixed 9x9 board
// Space Complexity: O(81) = O(1)

/**
 * @param {character[][]} board
 * @return {boolean}
 */
var isValidSudoku = function(board) {
    let rows = Array.from({ length: 9 }, () => Array(9).fill(false));
    let cols = Array.from({ length: 9 }, () => Array(9).fill(false));
    let boxes = Array.from({ length: 9 }, () => Array(9).fill(false));

    // Iterate through the board
    for (let i = 0; i < 9; i++) {
        for (let j = 0; j < 9; j++) {
            if (board[i][j] !== '.') {
                let num = board[i][j] - '1'; // Map '1'-'9' to 0-8
                let boxIndex = Math.floor(i / 3) * 3 + Math.floor(j / 3);

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
};
```

