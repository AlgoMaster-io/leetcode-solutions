# [36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

## Approach 1: Using HashSets for Rows, Columns, and Sub-boxes

### Solution
```cpp
// Time Complexity: O(9^2) = O(81)
// Space Complexity: O(9 * 9 * 3) = O(243) ≈ O(1) since the board size is constant
#include <vector>
#include <unordered_set>
#include <string>

class Solution {
public:
    bool isValidSudoku(std::vector<std::vector<char>>& board) {
        std::unordered_set<std::string> seen;

        // Iterate through the board
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char num = board[i][j];

                // Ignore empty cells
                if (num != '.') {
                    // Construct unique keys for rows, columns, and boxes
                    std::string rowKey = "row" + std::to_string(i) + num;
                    std::string colKey = "col" + std::to_string(j) + num;
                    std::string boxKey = "box" + std::to_string(i / 3) + std::to_string(j / 3) + num;

                    // Check if the keys already exist
                    if (!seen.insert(rowKey).second || !seen.insert(colKey).second || !seen.insert(boxKey).second) {
                        return false;
                    }
                }
            }
        }

        return true;
    }
};
```

## Approach 2: Using Arrays for Optimization

### Solution
```cpp
// Time Complexity: O(81) = O(1) for a fixed 9x9 board
// Space Complexity: O(81) = O(1)
#include <vector>

class Solution {
public:
    bool isValidSudoku(std::vector<std::vector<char>>& board) {
        bool rows[9][9] = {false};
        bool cols[9][9] = {false};
        bool boxes[9][9] = {false};

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
};
```

