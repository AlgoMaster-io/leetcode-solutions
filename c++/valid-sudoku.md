## [Leetcode Problem 36: Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

### Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach Using Hash Sets](#optimized-approach-using-hash-sets)

---

### Brute Force Approach

This approach systematically checks each row, column, and 3x3 grid to ensure that no duplicate numbers exist. By iterating over each number in the Sudoku board, we validate its placement according to Sudoku rules.

#### Intuition

- **Rows**: For each row, ensure no duplicate numbers.
- **Columns**: For each column, ensure no duplicate numbers.
- **Sub-boxes**: For each 3x3 sub-box, ensure no duplicate numbers.
  
This approach iteratively checks each row, column, and sub-box, making it straightforward but not the most efficient in terms of performance.

#### Solution

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        // Check each row
        for (int i = 0; i < 9; i++) {
            vector<int> count(9, 0); // Counter for numbers 1-9
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') {
                    int num = board[i][j] - '1'; // Convert char to index
                    count[num]++;
                    if (count[num] > 1) return false; // Duplicate found
                }
            }
        }
        
        // Check each column
        for (int j = 0; j < 9; j++) {
            vector<int> count(9, 0); // Counter for numbers 1-9
            for (int i = 0; i < 9; i++) {
                if (board[i][j] != '.') {
                    int num = board[i][j] - '1'; // Convert char to index
                    count[num]++;
                    if (count[num] > 1) return false; // Duplicate found
                }
            }
        }
        
        // Check each 3x3 sub-box
        for (int block = 0; block < 9; block++) {
            vector<int> count(9, 0); // Counter for numbers 1-9
            for (int i = block / 3 * 3; i < block / 3 * 3 + 3; i++) {
                for (int j = block % 3 * 3; j < block % 3 * 3 + 3; j++) {
                    if (board[i][j] != '.') {
                        int num = board[i][j] - '1'; // Convert char to index
                        count[num]++;
                        if (count[num] > 1) return false; // Duplicate found
                    }
                }
            }
        }
        
        return true; // No duplicates found, valid Sudoku
    }
};
```

- **Time Complexity**: O(1), since the board size is fixed (9x9).
- **Space Complexity**: O(1), since we use a fixed number of counting arrays regardless of input size.

### Optimized Approach Using Hash Sets

This approach uses hash sets to track the presence of numbers in rows, columns, and sub-boxes with a single scan of the board.

#### Intuition

Instead of iterating over each section separately, we use hash sets to record numbers as we traverse the board once. This method reduces redundant iteration and keeps the logic cleaner.

#### Solution

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        // Sets for rows, columns, and sub-boxes
        vector<unordered_set<char>> rows(9), columns(9), boxes(9);
        
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char num = board[i][j];
                if (num != '.') {
                    // Hashing logic for 3x3 sub-boxes
                    int boxIndex = (i / 3) * 3 + j / 3;
                    
                    // Check for duplicates
                    if (rows[i].count(num) || columns[j].count(num) || boxes[boxIndex].count(num)) {
                        return false;
                    }
                    
                    // Insert into sets
                    rows[i].insert(num);
                    columns[j].insert(num);
                    boxes[boxIndex].insert(num);
                }
            }
        }
        
        return true;
    }
};
```

- **Time Complexity**: O(1), since we process each cell of the fixed-size board once.
- **Space Complexity**: O(1), although we use sets, the board’s size is fixed, and so is the size of each set.

By using hash sets, we achieve a cleaner and more intuitive solution that maintains the time complexity while optimizing traversal redundancy.

