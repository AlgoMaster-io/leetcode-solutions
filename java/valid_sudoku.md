# [36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

## Approach 1: Using HashSets for Rows, Columns, and Sub-boxes

### Solution
```java
// Time Complexity: O(9^2) = O(81)
// Space Complexity: O(9 * 9 * 3) = O(243) ≈ O(1) since the board size is constant
import java.util.HashSet;

public class Solution {
    public boolean isValidSudoku(char[][] board) {
        HashSet<String> seen = new HashSet<>();

        // Iterate through the board
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char num = board[i][j];

                // Ignore empty cells
                if (num != '.') {
                    // Construct unique keys for rows, columns, and boxes
                    String rowKey = "row" + i + num;
                    String colKey = "col" + j + num;
                    String boxKey = "box" + (i / 3) + (j / 3) + num;

                    // Check if the keys already exist
                    if (!seen.add(rowKey) || !seen.add(colKey) || !seen.add(boxKey)) {
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
```java
// Time Complexity: O(81) = O(1) for a fixed 9x9 board
// Space Complexity: O(81) = O(1)
public class Solution {
    public boolean isValidSudoku(char[][] board) {
        boolean[][] rows = new boolean[9][9];
        boolean[][] cols = new boolean[9][9];
        boolean[][] boxes = new boolean[9][9];

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