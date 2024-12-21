# [LeetCode Problem 289: Game of Life](https://leetcode.com/problems/game-of-life/)

## Approaches
- [Approach 1: Copy of the Board and Basic Simulation](#approach-1-copy-of-the-board-and-basic-simulation)
- [Approach 2: In-Place State Tracking with Integer Encoding](#approach-2-in-place-state-tracking-with-integer-encoding)

---

## Approach 1: Copy of the Board and Basic Simulation

### Intuition
The problem involves updating the board according to the rules given, where a cell's next state is conditioned by its current state and the states of its eight neighbors. A straightforward approach is to create a copy of the board to hold the previous state, which can be referenced while we iterate over the original board to apply the game rules.

### Code

```java
public class Solution {
    public void gameOfLife(int[][] board) {
        if (board == null || board.length == 0) return;
        
        int m = board.length, n = board[0].length;
        int[][] copyBoard = new int[m][n];
        
        // Copy the original board to a new board
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                copyBoard[i][j] = board[i][j];
            }
        }
        
        // Directions array to get the neighbors (8 directions)
        int[] neighbors = {-1, 0, 1};
        
        // Iterate through board cell by cell
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int liveNeighbors = 0;

                // Check all 8 neighbors
                for (int x : neighbors) {
                    for (int y : neighbors) {
                        if (!(x == 0 && y == 0)) {
                            int r = i + x;
                            int c = j + y;

                            // Check the validity of the neighboring cell and if it was originally a live cell
                            if ((r >= 0 && r < m) && (c >= 0 && c < n) && copyBoard[r][c] == 1) {
                                liveNeighbors++;
                            }
                        }
                    }
                }
                
                // Apply the rules of the Game of Life to determine the next state of the cell
                if ((copyBoard[i][j] == 1) && (liveNeighbors < 2 || liveNeighbors > 3)) {
                    board[i][j] = 0;
                }
                if ((copyBoard[i][j] == 0) && liveNeighbors == 3) {
                    board[i][j] = 1;
                }
            }
        }
    }
}
```

### Complexity Analysis
- **Time Complexity:** \(O(m \times n)\), where \(m\) is the number of rows and \(n\) is the number of columns. We iterate over each cell and its neighbors.
- **Space Complexity:** \(O(m \times n)\), for the copy of the board.

---

## Approach 2: In-Place State Tracking with Integer Encoding

### Intuition
To optimize space usage, we can encode information about both current and next states in the same board. By using integer encoding, we can keep the board's state transitions in a single space. For instance, we can use different integers to represent current and next states:
- `0` -> `0`: 0 (Dead to Dead)
- `1` -> `1`: 1 (Live to Live)
- `1` -> `0`: 2 (Live to Dead)
- `0` -> `1`: 3 (Dead to Live)

### Code

```java
public class Solution {
    public void gameOfLife(int[][] board) {
        if (board == null || board.length == 0) return;
        
        int m = board.length, n = board[0].length;
        
        int[] neighbors = {-1, 0, 1};

        // Iterate through board cell by cell
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int liveNeighbors = 0;

                // Check all 8 neighbors
                for (int x : neighbors) {
                    for (int y : neighbors) {
                        if (!(x == 0 && y == 0)) {
                            int r = i + x;
                            int c = j + y;
                            // Use modulo operation to access previous state
                            if ((r >= 0 && r < m) && (c >= 0 && c < n) && (board[r][c] % 2 == 1)) {
                                liveNeighbors++;
                            }
                        }
                    }
                }
                
                // Apply the rules of the Game of Life
                if (board[i][j] == 1 && (liveNeighbors < 2 || liveNeighbors > 3)) {
                    board[i][j] = 2; // Live to Dead
                }
                if (board[i][j] == 0 && liveNeighbors == 3) {
                    board[i][j] = 3; // Dead to Live
                }
            }
        }

        // Finalize the next state
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = board[i][j] % 2;
            }
        }
    }
}
```

### Complexity Analysis
- **Time Complexity:** \(O(m \times n)\), where \(m\) is the number of rows and \(n\) is the number of columns. Each cell and its neighbors are checked.
- **Space Complexity:** \(O(1)\), as we are modifying the board in-place without using extra space.

