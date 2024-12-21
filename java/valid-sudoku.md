# [Leetcode 36: Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach Using HashSet](#optimized-approach-using-hashset)

---

### Brute Force Approach

**Intuition:**

The Brute Force approach involves checking all rows, columns, and sub-grids (3x3) to ensure they contain unique numbers. This is straightforward but can be inefficient due to repeated checks.

**Steps:**

1. Check each row to make sure there are no repeated numbers.
2. Check each column and ensure no duplicates.
3. Check each 3x3 sub-grid.

**Implementation:**

```java
public class Solution {
    public boolean isValidSudoku(char[][] board) {
        // Check each row
        for (int i = 0; i < 9; i++) {
            if (!isValidUnit(board[i])) {
                return false;
            }
        }

        // Check each column
        for (int j = 0; j < 9; j++) {
            char[] column = new char[9];
            for (int i = 0; i < 9; i++) {
                column[i] = board[i][j];
            }
            if (!isValidUnit(column)) {
                return false;
            }
        }

        // Check each 3x3 grid
        for (int block = 0; block < 9; block++) {
            char[] blockArray = new char[9];
            int rowStart = 3 * (block / 3);
            int colStart = 3 * (block % 3);
            int index = 0;
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    blockArray[index++] = board[rowStart + i][colStart + j];
                }
            }
            if (!isValidUnit(blockArray)) {
                return false;
            }
        }

        return true;
    }

    private boolean isValidUnit(char[] unit) {
        boolean[] seen = new boolean[9];
        for (char num : unit) {
            if (num != '.') {
                int index = num - '1';
                if (seen[index]) {
                    return false;  // If already seen, it's a duplicate
                }
                seen[index] = true;  // Mark the number as seen
            }
        }
        return true;
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(9^2) = O(81). Each row, column, and grid is checked separately.
- **Space Complexity:** O(1) for each row and O(9) for columns and blocks used by boolean arrays.

---

### Optimized Approach Using HashSet

**Intuition:**

To optimize, use a HashSet for keeping track of seen numbers efficiently. We iterate once through the board, and at each step, we ensure that no number appears twice in its row, column or sub-grid.

**Steps:**

1. A single loop performs checks using HashSet for:
   - Rows
   - Columns
   - 3x3 sub-grids
2. For each filled cell, create unique keys for HashSets.

**Implementation:**

```java
import java.util.HashSet;

public class Solution {
    public boolean isValidSudoku(char[][] board) {
        HashSet<String> seen = new HashSet<>();

        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char currentValue = board[i][j];
                if (currentValue != '.') {
                    // For each value, ensure it is unique in its row, column, and sub-grid
                    if (!seen.add(currentValue + " found in row " + i) ||
                        !seen.add(currentValue + " found in column " + j) ||
                        !seen.add(currentValue + " found in subgrid " + i/3 + "-" + j/3)) {
                        return false;  // If any of the adds return false, the board isn't valid
                    }
                }
            }
        }
        return true;
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(1). We are processing a constant number of cells in a 9x9 grid.
- **Space Complexity:** O(1). The size of the HashSet used will hold at most 81 elements given all cells are filled without repetition.

In this solution, we efficiently ensure that no duplicate values exist in rows, columns, or 3x3 sub-grids in a single pass through the board, utilizing the properties of the HashSet for fast membership checks.

