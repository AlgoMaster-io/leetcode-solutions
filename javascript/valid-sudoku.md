# [Leetcode 36: Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach Using Hash Sets](#optimized-approach-using-hash-sets)

### Brute Force Approach

**Intuition**:  
The simplest way to solve the problem is to check every row, column, and 3x3 sub-grid (or box) of the board. In this approach, we will iterate through each of these areas and use arrays to track the numbers we've seen. If we encounter a repeated number, we'll determine that the board is invalid.

**Steps**:
1. Use three 2D arrays `rows`, `cols`, and `boxes` to keep track of numbers seen in each row, column, and box.
2. Iterate over each cell in the board.
3. If a cell contains a digit, convert it from character to integer.
4. Check if the number has already appeared in the corresponding row, column, or box. If so, return false.
5. If not, mark the number as seen in the respective row, column, and box.
6. If we check all cells without finding duplicates, return true.

**Code**:
```javascript
var isValidSudoku = function(board) {
    // Create three arrays to track numbers in rows, columns, and boxes
    let rows = Array(9).fill(0).map(() => Array(9).fill(false));
    let cols = Array(9).fill(0).map(() => Array(9).fill(false));
    let boxes = Array(9).fill(0).map(() => Array(9).fill(false));
    
    for (let r = 0; r < 9; r++) {
        for (let c = 0; c < 9; c++) {
            if (board[r][c] !== '.') {
                // Convert char to number
                let num = board[r][c] - '0' - 1; // '-1' because 0 indexing
                let k = Math.floor(r / 3) * 3 + Math.floor(c / 3);
                
                // Check for duplicates
                if (rows[r][num] || cols[c][num] || boxes[k][num]) {
                    return false;
                }
                
                // Mark the number as seen
                rows[r][num] = true;
                cols[c][num] = true;
                boxes[k][num] = true;
            }
        }
    }
    return true;
};
```

**Time Complexity**: O(1) since the board is always a 9x9 matrix.  
**Space Complexity**: O(1) since the space required to store up to 27 arrays (9x3) is constant and does not grow with input size.

---

### Optimized Approach Using Hash Sets

**Intuition**:  
While the brute force approach using arrays for tracking is straightforward, we can optimize the space by using hash sets. The sets automatically handle the uniqueness of numbers and are more suitable for handling sparse data.

**Steps**:
1. Use three sets `rows`, `cols`, and `boxes` to keep track of numbers seen in each row, column, and box.
2. Iterate over each cell in the board.
3. If a cell contains a digit, compute which row, column, and box set to check.
4. Use strings to represent the number presence in `rows`, `cols`, and `boxes` and check for conflicts.
5. If no duplicates are found by the end of iteration, return true.

**Code**:
```javascript
var isValidSudoku = function(board) {
    // Create sets to track numbers in rows, columns, and boxes
    let rows = Array.from({ length: 9 }, () => new Set());
    let cols = Array.from({ length: 9 }, () => new Set());
    let boxes = Array.from({ length: 9 }, () => new Set());
    
    for (let r = 0; r < 9; r++) {
        for (let c = 0; c < 9; c++) {
            if (board[r][c] !== '.') {
                let num = board[r][c];
                let boxIndex = Math.floor(r / 3) * 3 + Math.floor(c / 3);
                
                // Check for duplicates using hash sets
                if (rows[r].has(num) || cols[c].has(num) || boxes[boxIndex].has(num)) {
                    return false;
                }
                
                // Add the number to the respective sets
                rows[r].add(num);
                cols[c].add(num);
                boxes[boxIndex].add(num);
            }
        }
    }
    return true;
};
```

**Time Complexity**: O(1), since we are iterating over a fixed grid.  
**Space Complexity**: O(1), as the use of hash sets for fixed-size data (9x3 sets) remains constant.

