# [Leetcode 289: Game of Life](https://leetcode.com/problems/game-of-life/)

## Approaches

- [Approach 1: Brute Force with Extra Space](#approach-1-brute-force-with-extra-space)
- [Approach 2: In-place Simulation with State Encoding](#approach-2-in-place-simulation-with-state-encoding)

---

## Approach 1: Brute Force with Extra Space

### Intuition
The Game of Life is a simple simulation based on neighbor counting. The brute force approach involves creating a copy of the board to calculate the next state without modifying the original state prematurely.

### Steps
1. Create a new board of the same size to store the next state.
2. For each cell, determine the number of live neighbors.
3. Apply the rules of the game to decide the next state of the cell and store it in the new board.
4. Once all cells are updated, copy the new board back to the original board.

### Code
```cpp
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int m = board.size(), n = board[0].size();
        vector<vector<int>> nextState = board;

        // Directions array for 8 neighbors
        vector<vector<int>> directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}, 
                                          {1, 1}, {-1, -1}, {1, -1}, {-1, 1}};
        
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int liveNeighbors = 0;
                for (auto dir : directions) {
                    int ni = i + dir[0], nj = j + dir[1];
                    if (ni >= 0 && ni < m && nj >= 0 && nj < n && board[ni][nj] == 1) {
                        liveNeighbors++;
                    }
                }

                // Apply Game of Life rules
                if (board[i][j] == 1) {
                    if (liveNeighbors < 2 || liveNeighbors > 3) {
                        nextState[i][j] = 0;
                    }
                } else {
                    if (liveNeighbors == 3) {
                        nextState[i][j] = 1;
                    }
                }
            }
        }

        // Update the original board
        board = nextState;
    }
};
```

### Complexity
- **Time Complexity**: \(O(m \times n)\), where \(m\) and \(n\) are the dimensions of the board, since we check every cell and its neighbors.
- **Space Complexity**: \(O(m \times n)\), due to the additional board used to store the next state.

---

## Approach 2: In-place Simulation with State Encoding

### Intuition
Instead of using additional space, we can encode the state changes directly in the existing board by using different values to represent transitions without requiring a second board.

### Steps
1. Traverse each cell to count its live neighbors.
2. Use encoding to represent state changes.
   - Encode "0 → 1" as 2.
   - Encode "1 → 0" as 3.
3. Apply the rules and encode states directly in the board.
4. Decode the board back to 0s and 1s after processing all cells.

### Code
```cpp
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int m = board.size(), n = board[0].size();
        vector<vector<int>> directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}, 
                                          {1, 1}, {-1, -1}, {1, -1}, {-1, 1}};

        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int liveNeighbors = 0;
                for (auto dir : directions) {
                    int ni = i + dir[0], nj = j + dir[1];
                    if (ni >= 0 && ni < m && nj >= 0 && nj < n && (board[ni][nj] == 1 || board[ni][nj] == 3)) {
                        liveNeighbors++;
                    }
                }

                // Apply the rules of the game
                if (board[i][j] == 1) {
                    if (liveNeighbors < 2 || liveNeighbors > 3) {
                        board[i][j] = 3; // Live -> Dead
                    }
                } else {
                    if (liveNeighbors == 3) {
                        board[i][j] = 2; // Dead -> Live
                    }
                }
            }
        }

        // Decode the board back to original states
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (board[i][j] == 2) {
                    board[i][j] = 1;
                } else if (board[i][j] == 3) {
                    board[i][j] = 0;
                }
            }
        }
    }
};
```

### Complexity
- **Time Complexity**: \(O(m \times n)\), processing each cell and its neighbors.
- **Space Complexity**: \(O(1)\), as the board is modified in place without extra space. 

The second approach is generally preferred for its reduced space complexity and efficient use of state encoding.

