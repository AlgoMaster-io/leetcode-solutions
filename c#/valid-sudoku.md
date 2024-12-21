# [Leetcode 36: Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

## Approaches
- [Approach 1: Check Rows, Columns, and Boxes Directly](#approach-1)
- [Approach 2: Optimized HashSet Solution](#approach-2)

---

## Approach 1: Check Rows, Columns, and Boxes Directly

### Intuition
The idea is to check all rows, columns, and sub-boxes separately to ensure there are no duplicates of numbers between '1' to '9'. We'll loop through each cell and verify the constraints for Sudoku validity.

### Steps
1. Use three separate arrays to track numbers in each row, column, and sub-box (3x3 section of the board).
2. For each number in a cell, mark it in the corresponding row, column, and sub-box array.
3. If a number is already marked for a row, column, or box, the Sudoku board is invalid.
4. Traverse each cell and apply these checks.

### Code
```csharp
public class Solution {
    public bool IsValidSudoku(char[][] board) {
        // Initialize arrays for rows, columns, and boxes
        bool[,] rows = new bool[9, 9];
        bool[,] columns = new bool[9, 9];
        bool[,] boxes = new bool[9, 9];

        for (int r = 0; r < 9; r++) {
            for (int c = 0; c < 9; c++) {
                // Skip empty cells
                if (board[r][c] == '.') continue;

                // Calculate digit to mark (adjusted to 0-indexed)
                int digit = board[r][c] - '1';

                // Calculate box index
                int boxIndex = (r / 3) * 3 + (c / 3);

                // Check if the digit already marked in row, column, or box
                if (rows[r, digit] || columns[c, digit] || boxes[boxIndex, digit])
                    return false;
                
                // Mark the digit in the current row, column, and box
                rows[r, digit] = true;
                columns[c, digit] = true;
                boxes[boxIndex, digit] = true;
            }
        }

        // If no duplication is found, the board is valid
        return true;
    }
}
```

### Complexity
- **Time Complexity**: \(O(1)\) because we are parsing a fixed 81-cell board (9x9).
- **Space Complexity**: \(O(1)\) because the extra space used (for rows, columns, and boxes arrays) does not scale with the input size.

---

## Approach 2: Optimized HashSet Solution

### Intuition
An alternative method uses hash sets to store seen numbers for each row, column, and sub-box. This approach simplifies the check and marking process by using dynamic collections.

### Steps
1. Utilize hash sets to store and detect duplicates in rows, columns, and boxes.
2. Traverse each cell on the board:
   - If the cell contains a number, add it to corresponding sets.
   - If a number is already present in any set, return false.

### Code
```csharp
public class Solution {
    public bool IsValidSudoku(char[][] board) {
        // Initialize hash sets for rows, columns, and boxes
        HashSet<char>[] rows = new HashSet<char>[9];
        HashSet<char>[] columns = new HashSet<char>[9];
        HashSet<char>[] boxes = new HashSet<char>[9];
        
        // Initialize each hash set
        for (int i = 0; i < 9; i++) {
            rows[i] = new HashSet<char>();
            columns[i] = new HashSet<char>();
            boxes[i] = new HashSet<char>();
        }

        for (int r = 0; r < 9; r++) {
            for (int c = 0; c < 9; c++) {
                char current = board[r][c];
                
                if (current == '.') continue;
                
                int boxIndex = (r / 3) * 3 + (c / 3);

                // Check if number is already in row, column or box
                if (rows[r].Contains(current) || columns[c].Contains(current) || boxes[boxIndex].Contains(current))
                    return false;

                // Add number to respective row, column, and box sets
                rows[r].Add(current);
                columns[c].Add(current);
                boxes[boxIndex].Add(current);
            }
        }

        // No duplicates found, the Sudoku board is valid
        return true;
    }
}
```

### Complexity
- **Time Complexity**: \(O(1)\), similar to the previous approach because of fixed board size.
- **Space Complexity**: \(O(1)\) as the maximum number of extra space is bound by the board size constants (9 sets of size at most 9). 

---


