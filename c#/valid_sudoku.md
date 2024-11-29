# [36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

## Approach 1: Using HashSets for Rows, Columns, and Sub-boxes

### Solution
csharp
```csharp
// Time Complexity: O(9^2) = O(81)
// Space Complexity: O(9 * 9 * 3) = O(243) ≈ O(1) since the board size is constant
using System.Collections.Generic;

public class Solution {
    public bool IsValidSudoku(char[][] board) {
        HashSet<string> seen = new HashSet<string>();

        // Iterate through the board
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char num = board[i][j];

                // Ignore empty cells
                if (num != '.') {
                    // Construct unique keys for rows, columns, and boxes
                    string rowKey = "row" + i + num;
                    string colKey = "col" + j + num;
                    string boxKey = "box" + (i / 3) + (j / 3) + num;

                    // Check if the keys already exist
                    if (!seen.Add(rowKey) || !seen.Add(colKey) || !seen.Add(boxKey)) {
                        return false;
                    }
                }
            }
        }

        return true;
    }
}
```

## Approach 2: Using Arrays for Optimization

### Solution
csharp
```csharp
// Time Complexity: O(81) = O(1) for a fixed 9x9 board
// Space Complexity: O(81) = O(1)
public class Solution {
    public bool IsValidSudoku(char[][] board) {
        bool[][] rows = new bool[9][];
        bool[][] cols = new bool[9][];
        bool[][] boxes = new bool[9][];

        for (int i = 0; i < 9; i++) {
            rows[i] = new bool[9];
            cols[i] = new bool[9];
            boxes[i] = new bool[9];
        }

        // Iterate through the board
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') {
                    int num = board[i][j] - '1'; // Map '1'-'9' to 0-8
                    int boxIndex = (i / 3) * 3 + (j / 3);

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
}
```

