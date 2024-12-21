## LeetCode Problem: [36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

### Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Single Pass with HashSets](#single-pass-with-hashsets)

---

### Brute Force Approach

#### Intuition:
The brute force approach involves checking every row, column, and 3x3 sub-box individually to ensure there are no duplicate numbers. This can be achieved by using data structures like arrays or sets to track the numbers we have seen so far.

#### Approach:
1. Iterate over each cell in the board.
2. For each non-empty cell, check if the number violates Sudoku rules in:
   - The corresponding row.
   - The corresponding column.
   - The corresponding 3x3 sub-box.
3. Utilize arrays to keep track of which numbers have been encountered in each row, column, and sub-box.
4. If a duplicate is found in any category, return false.
5. If the loop completes without finding any duplicates, return true.

#### Code:
```typescript
function isValidSudoku(board: string[][]): boolean {
    // Initialize arrays to store the state of numbers in rows, columns and sub-boxes
    const rows: Set<number>[] = Array.from({ length: 9 }, () => new Set<number>());
    const cols: Set<number>[] = Array.from({ length: 9 }, () => new Set<number>());
    const boxes: Set<number>[] = Array.from({ length: 9 }, () => new Set<number>());

    // Iterate through the board
    for (let r = 0; r < 9; r++) {
        for (let c = 0; c < 9; c++) {
            const value = board[r][c];

            // Skip empty cells
            if (value === '.') continue;

            const num = Number(value);
            const boxIndex = Math.floor(r / 3) * 3 + Math.floor(c / 3);

            // Check for duplicates in row, column, and box
            if (rows[r].has(num) || cols[c].has(num) || boxes[boxIndex].has(num)) {
                return false;
            }

            // Add the number to the respective row, column, and box sets
            rows[r].add(num);
            cols[c].add(num);
            boxes[boxIndex].add(num);
        }
    }

    return true;
}
```

#### Time and Space Complexity:
- **Time Complexity:** \(O(N^2)\), where \(N\) is the number of cells in the board. We process each cell once.
- **Space Complexity:** \(O(N)\), due to storing state in rows, columns, and boxes for numbers 1-9.

---

### Single Pass with HashSets

#### Intuition:
By using hash sets, we can optimize our approach by verifying row, column, and sub-box constraints in a single pass through the board. This reduces overhead and keeps the logic clear and concise.

#### Approach:
1. Traverse each cell in a single iteration.
2. Employ hash sets to track numbers seen across rows, columns, and boxes simultaneously.
3. Use mathematical operations to determine the index of the sub-box.
4. As soon as a duplicate number is identified within any collection, return false.
5. If no conflicts are encountered throughout, the board is valid.

#### Code:
```typescript
function isValidSudoku(board: string[][]): boolean {
    const seen = new Set<string>();

    // Traverse each cell in the board
    for (let i = 0; i < 9; ++i) {
        for (let j = 0; j < 9; ++j) {
            const currentValue = board[i][j];
            
            // Skip empty cells
            if (currentValue === '.') continue;

            // Construct unique keys for rows, columns, and boxes
            const rowKey = `row ${i} value ${currentValue}`;
            const columnKey = `column ${j} value ${currentValue}`;
            const boxKey = `box ${Math.floor(i / 3)}-${Math.floor(j / 3)} value ${currentValue}`;

            // Check if any of these keys have been seen before, indicating a duplicate
            if (seen.has(rowKey) || seen.has(columnKey) || seen.has(boxKey)) {
                return false;
            }

            // Add the keys to the set as they were not previously seen
            seen.add(rowKey);
            seen.add(columnKey);
            seen.add(boxKey);
        }
    }
    
    return true;
}
```

#### Time and Space Complexity:
- **Time Complexity:** \(O(N^2)\), where \(N\) is the number of cells, as every cell is processed once.
- **Space Complexity:** \(O(N)\), for the hash set holding unique string representations up to the number of values processed.

