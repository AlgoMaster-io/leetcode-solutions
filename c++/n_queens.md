# 51. [N-Queens](https://leetcode.com/problems/n-queens/)

## Approach 1: Backtracking

### Solution
```cpp
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n^2) for storing the board state
#include <vector>
#include <string>

class Solution {
public:
    std::vector<std::vector<std::string>> solveNQueens(int n) {
        std::vector<std::vector<std::string>> result;
        std::vector<std::string> board(n, std::string(n, '.'));
        backtrack(board, 0, result);
        return result;
    }

private:
    void backtrack(std::vector<std::string>& board, int row, std::vector<std::vector<std::string>>& result) {
        if (row == board.size()) {
            result.push_back(board); // Add the current board configuration to the result
            return;
        }

        for (int col = 0; col < board.size(); ++col) {
            if (isSafe(board, row, col)) {
                board[row][col] = 'Q'; // Place the queen
                backtrack(board, row + 1, result); // Recurse to the next row
                board[row][col] = '.'; // Backtrack and remove the queen
            }
        }
    }

    bool isSafe(const std::vector<std::string>& board, int row, int col) {
        // Check the column
        for (int i = 0; i < row; ++i) {
            if (board[i][col] == 'Q') {
                return false;
            }
        }

        // Check the diagonal (top-left to bottom-right)
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; --i, --j) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }

        // Check the anti-diagonal (top-right to bottom-left)
        for (int i = row - 1, j = col + 1; i >= 0 && j < board.size(); --i, ++j) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }

        return true; // The position is safe for a queen
    }
};
```

## Approach 2: Optimized Backtracking with Bitmasking

### Solution
```cpp
// Time Complexity: O(n!), where n is the size of the board
// Space Complexity: O(n) for storing the column, diagonal, and anti-diagonal states
#include <vector>
#include <string>

class Solution {
public:
    std::vector<std::vector<std::string>> solveNQueens(int n) {
        std::vector<std::vector<std::string>> result;
        std::vector<int> queens(n, -1);
        backtrack(0, n, queens, result);
        return result;
    }

private:
    void backtrack(int row, int n, std::vector<int>& queens, std::vector<std::vector<std::string>>& result) {
        if (row == n) {
            result.push_back(construct(queens, n)); // Add the current board configuration to the result
            return;
        }

        for (int col = 0; col < n; ++col) {
            if (isSafe(row, col, queens)) {
                queens[row] = col; // Place the queen in the current row
                backtrack(row + 1, n, queens, result); // Recurse to the next row
                queens[row] = -1; // Backtrack
            }
        }
    }

    bool isSafe(int row, int col, const std::vector<int>& queens) {
        for (int i = 0; i < row; ++i) {
            if (queens[i] == col || std::abs(queens[i] - col) == std::abs(i - row)) { 
                return false; // Check for column and diagonal conflicts
            }
        }
        return true;
    }

    std::vector<std::string> construct(const std::vector<int>& queens, int n) {
        std::vector<std::string> board(n, std::string(n, '.'));
        for (int i = 0; i < n; ++i) {
            board[i][queens[i]] = 'Q';
        }
        return board;
    }
};
```

