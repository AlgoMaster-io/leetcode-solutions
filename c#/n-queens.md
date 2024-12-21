# [Leetcode 51: N-Queens](https://leetcode.com/problems/n-queens/)

## Solutions

1. [Basic Backtracking](#basic-backtracking)
2. [Optimized Backtracking with HashSets](#optimized-backtracking-with-hashsets)

## 1. Basic Backtracking

### Intuition
The N-Queens problem requires placing N queens on an N×N chessboard such that none of them attacks each other. This can be approached using backtracking, a common technique for exploring all possible configurations in a search space. We'll simulate placing queens row by row and use the principle of backtracking to revert and try new configurations when a conflict is detected.

### Approach
1. Create a board represented by a 2D char array initialized with '.' indicating empty spaces.
2. Start placing queens row by row.
3. For each position in the current row, check if it is safe to place a queen.
4. A position is considered safe if no queen exists in the same column, or diagonals.
5. If a position is safe, place the queen and move to the next row.
6. If all queens are successfully placed, add the board configuration to the results.
7. Backtrack: if placing a queen leads to no solution, remove it and try the next position.

### Code
```csharp
public class Solution {
    public IList<IList<string>> SolveNQueens(int n) {
        IList<IList<string>> result = new List<IList<string>>();
        char[][] board = new char[n][];
        for (int i = 0; i < n; i++) {
            board[i] = new string('.', n).ToCharArray();
        }
        
        Solve(0, board, result);
        return result;
    }

    private void Solve(int row, char[][] board, IList<IList<string>> result) {
        int n = board.Length;
        
        // If all queens are placed, convert the board to a list of strings and add to results
        if (row == n) {
            result.Add(board.Select(r => new string(r)).ToList());
            return;
        }
        
        // Try placing a queen in each column of the current row
        for (int col = 0; col < n; col++) {
            if (IsSafe(board, row, col)) {
                board[row][col] = 'Q'; // Place queen
                Solve(row + 1, board, result); // Move to next row
                board[row][col] = '.'; // Backtrack
            }
        }
    }

    private bool IsSafe(char[][] board, int row, int col) {
        int n = board.Length;
        
        // Check column
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q') return false;
        }
        
        // Check diagonal top-left
        for (int i=row-1, j=col-1; i>=0 && j>=0; i--, j--) {
            if (board[i][j] == 'Q') return false;
        }
        
        // Check diagonal top-right
        for (int i=row-1, j=col+1; i>=0 && j<n; i--, j++) {
            if (board[i][j] == 'Q') return false;
        }
        
        return true;
    }
}
```

### Time Complexity
- O(N!) because the search space is potentially all permutations of N queens in N rows.

### Space Complexity
- O(N^2) for the board and potentially the recursion stack.

## 2. Optimized Backtracking with HashSets

### Intuition
To make the solution more efficient, we can utilize additional data structures to keep track of columns and diagonals where queens are already placed. This reduces the need to iterate over the board to check for attacks and makes the position checking O(1).

### Approach
1. Use three HashSets to track attacked columns, major (left) diagonals, and minor (right) diagonals.
2. Use these sets to check if a queen's position is safe in constant time.
3. Continue with backtracking similarly by placing queens and storing results.

### Code
```csharp
public class Solution {
    public IList<IList<string>> SolveNQueens(int n) {
        IList<IList<string>> result = new List<IList<string>>();
        char[][] board = new char[n][];
        for (int i = 0; i < n; i++) {
            board[i] = new string('.', n).ToCharArray();
        }
        
        HashSet<int> cols = new HashSet<int>();
        HashSet<int> majorDiag = new HashSet<int>();
        HashSet<int> minorDiag = new HashSet<int>();

        Solve(0, board, result, cols, majorDiag, minorDiag);
        return result;
    }

    private void Solve(int row, char[][] board, IList<IList<string>> result,
                       HashSet<int> cols, HashSet<int> majorDiag, HashSet<int> minorDiag) {
        int n = board.Length;
        
        if (row == n) {
            result.Add(board.Select(r => new string(r)).ToList());
            return;
        }
        
        for (int col = 0; col < n; col++) {
            if (!cols.Contains(col) && !majorDiag.Contains(row - col) && !minorDiag.Contains(row + col)) {
                board[row][col] = 'Q';
                cols.Add(col);
                majorDiag.Add(row - col);
                minorDiag.Add(row + col);
                
                Solve(row + 1, board, result, cols, majorDiag, minorDiag);
                
                board[row][col] = '.';
                cols.Remove(col);
                majorDiag.Remove(row - col);
                minorDiag.Remove(row + col);
            }
        }
    }
}
```

### Time Complexity
- Still O(N!) because we're exploring permutations, but the constant factors are reduced due to O(1) lookups.

### Space Complexity
- O(N) for the extra HashSets and the recursion stack. The intermediate board still requires O(N^2).

This concludes the solutions for the N-Queens problem using C#.

