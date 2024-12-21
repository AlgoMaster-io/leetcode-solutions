# [Leetcode 289: Game of Life](https://leetcode.com/problems/game-of-life/)

## Approaches

1. [Brute Force Simulation](#brute-force-simulation)
2. [In-place State Change with Additional State](#in-place-state-change-with-additional-state)
3. [Optimized In-place State Change](#optimized-in-place-state-change)

---

## Brute Force Simulation

### Intuition

The game of life is straightforward simulation problem. The basic approach is to simulate the board state over each cell transition by following the rules. For this approach, the key is using a temporary board to store the updates, so that the updates don't affect each other during the simulation.

### Steps

1. Create a helper function to count the number of live neighbors.
2. Iterate over each cell and determine the next state of the cell according to the rules, storing the results in a new temporary board.
3. Copy the temporary board back to the original board.

```csharp
public void GameOfLife(int[][] board) {
    if(board == null || board.Length == 0) return;
    int rows = board.Length, cols = board[0].Length;
    int[][] tempBoard = new int[rows][];
    for(int i = 0; i < rows; i++)
        tempBoard[i] = (int[])board[i].Clone(); // Create a copy of the board

    // Define directions for the 8 neighbors
    int[] dx = {-1, -1, -1, 0, 0, 1, 1, 1};
    int[] dy = {-1, 0, 1, -1, 1, -1, 0, 1};

    for(int r = 0; r < rows; r++) {
        for(int c = 0; c < cols; c++) {
            int liveNeighbors = 0;
            // Count live neighbors
            for(int k = 0; k < 8; k++) {
                int newRow = r + dx[k], newCol = c + dy[k];
                if(newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols) {
                    liveNeighbors += board[newRow][newCol];
                }
            }
            // Apply the game rules
            if(board[r][c] == 1) {
                if(liveNeighbors < 2 || liveNeighbors > 3) tempBoard[r][c] = 0;
            } else {
                if(liveNeighbors == 3) tempBoard[r][c] = 1;
            }
        }
    }

    // Copy the temp board back to the original board
    for (int r = 0; r < rows; r++)
        for (int c = 0; c < cols; c++)
            board[r][c] = tempBoard[r][c];
}
```

**Time Complexity:** O(R * C), where R is the number of rows and C is the number of columns in the board.

**Space Complexity:** O(R * C) for the temporary board.

---

## In-place State Change with Additional State

### Intuition

This approach aims to reduce space complexity by embedding additional state information directly into the board. We use two bits per cell: the least significant bit to represent the current state and the second least significant bit to represent the next state.

### Steps

1. Use bit manipulation to simultaneously store both the current and next state of each cell.
2. Iterate over the board and determine the next state.
3. Apply the transition by moving the next state into the current state bits.

```csharp
public void GameOfLife(int[][] board) {
    if(board == null || board.Length == 0) return;
    int rows = board.Length, cols = board[0].Length;

    int[] dx = {-1, -1, -1, 0, 0, 1, 1, 1};
    int[] dy = {-1, 0, 1, -1, 1, -1, 0, 1};

    for(int r = 0; r < rows; r++) {
        for(int c = 0; c < cols; c++) {
            int liveNeighbors = 0;
            // Count live neighbors
            for(int k = 0; k < 8; k++) {
                int newRow = r + dx[k], newCol = c + dy[k];
                if(newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols) {
                    liveNeighbors += board[newRow][newCol] & 1;
                }
            }
            // Apply the game rules and encode the next state in the second bit
            if(board[r][c] == 1) {
                if(liveNeighbors == 2 || liveNeighbors == 3) board[r][c] |= 2;
            } else {
                if(liveNeighbors == 3) board[r][c] |= 2;
            }
        }
    }

    // Update the board to the next state
    for (int r = 0; r < rows; r++)
        for (int c = 0; c < cols; c++)
            board[r][c] >>= 1; // Move the second bit to the first bit position
}
```

**Time Complexity:** O(R * C), where R is the number of rows and C is the number of columns in the board.

**Space Complexity:** O(1) - In-place modification.

---

## Optimized In-place State Change

### Intuition

Building upon the in-place state change approach, instead of using two bits just for readability, directly use constants to indicate states: `2` for born and `-1` for dying. This helps shrink the readability logic to a single pass update, reducing unnecessary complexity in the handling of bits.

### Steps

1. Define new integer values to represent state transitions.
2. Process each cell and decide transitions based on live neighbor count.
3. Make one more pass to finalize transitions by switching to the actual next state.

```csharp
public void GameOfLife(int[][] board) {
    if(board == null || board.Length == 0) return;
    int rows = board.Length, cols = board[0].Length;

    int[] dx = {-1, -1, -1, 0, 0, 1, 1, 1};
    int[] dy = {-1, 0, 1, -1, 1, -1, 0, 1};

    for(int r = 0; r < rows; r++) {
        for(int c = 0; c < cols; c++) {
            int liveNeighbors = 0;
            // Count live neighbors
            for(int k = 0; k < 8; k++) {
                int newRow = r + dx[k], newCol = c + dy[k];
                if(newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols) {
                    liveNeighbors += Math.Abs(board[newRow][newCol]) == 1 ? 1 : 0;
                }
            }
            // Apply the game rules
            if(board[r][c] == 1) {
                if(liveNeighbors < 2 || liveNeighbors > 3) board[r][c] = -1;  // dying
            } else {
                if(liveNeighbors == 3) board[r][c] = 2;  // born
            }
        }
    }

    // Update the board to the next state
    for (int r = 0; r < rows; r++)
        for (int c = 0; c < cols; c++)
            if (board[r][c] > 0) board[r][c] = 1;
            else board[r][c] = 0;
}
```

**Time Complexity:** O(R * C), where R is the number of rows and C is the number of columns in the board.

**Space Complexity:** O(1) - In-place modification without additional space.

