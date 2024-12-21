# [LeetCode 51: N-Queens](https://leetcode.com/problems/n-queens/)

## Approaches:

1. [Backtracking](#backtracking)

## Backtracking

### Intuition

The N-Queens problem is a classic puzzle where the goal is to place N queens on an N×N chessboard such that no two queens threaten each other. This means no two queens should share the same row, column, or diagonal.

Since each queen must be placed in one of the N rows, and each row can only have one queen, the problem boils down to determining the column for each row such that no two queens conflict with each other. 

The backtracking approach allows us to explore all possible positions and backtrack when a position doesn't lead to a solution, thus exploring potential solutions recursively.

### Approach

1. We'll use a backtracking function that tries to place queens row by row.
2. For each row, we try placing a queen in each column. 
3. Before placing a queen at a position (row, col), we check if it's safe by ensuring no queens are placed in:
   - the same column,
   - the same major diagonal (i - j), and
   - the same minor diagonal (i + j).
4. If it's safe, we place the queen and recurse to the next row.
5. If we successfully place queens in all rows, then we've found one valid arrangement.
6. If not, we backtrack by removing the queen and trying the next column for that row.

### Implementation

```cpp
#include <vector>
#include <string>

using namespace std;

vector<vector<string>> solveNQueens(int n) {
    // To store final solutions
    vector<vector<string>> solutions;
    // Board to maintain the position of queens
    vector<string> board(n, string(n, '.'));
    // Vectors to mark the columns and diagonals
    vector<int> cols(n, 0), d1(2 * n, 0), d2(2 * n, 0);

    // Backtracking function
    function<void(int)> backtrack = [&](int row) {
        // If row is equal to n, a valid solution is found
        if (row == n) {
            solutions.push_back(board);
            return;
        }
        
        // Try placing the queen in each column on the current row
        for (int col = 0; col < n; ++col) {
            // Check if placing a queen here is safe
            if (cols[col] || d1[row - col + n] || d2[row + col]) continue;
            
            // Place the queen
            board[row][col] = 'Q';
            // Mark the column, and both diagonals
            cols[col] = d1[row - col + n] = d2[row + col] = 1;
            
            // Recurse to the next row
            backtrack(row + 1);
            
            // Backtrack: remove the queen and unmark
            board[row][col] = '.';
            cols[col] = d1[row - col + n] = d2[row + col] = 0;
        }
    };

    backtrack(0); // Start from the first row
    return solutions;
}
```

### Time Complexity

The time complexity for the backtracking approach is O(N!), since in the worst case, the algorithm will check all possible configurations.

### Space Complexity

The space complexity is O(N), used by the recursive call stack, and additional space is O(N^2) for storing the board representation and helper vectors for marking conflicts.

