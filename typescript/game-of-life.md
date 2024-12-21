## [Leetcode 289: Game of Life](https://leetcode.com/problems/game-of-life/)

- [Approach 1: Simulation with Copy of Board](#approach-1-simulation-with-copy-of-board)
- [Approach 2: In-place Simulation with Encoding States](#approach-2-in-place-simulation-with-encoding-states)

### Approach 1: Simulation with Copy of Board

To simulate the Game of Life on a 2D grid, we can first create a copy of the board. We then iterate over the original board and for each cell, count its live neighbors using the copy. Based on the rules given, we update the cell's state on the original board.

#### Intuition

1. **Create a Board Copy**: A copy of the original board helps us keep track of the initial states while updating cells based on their neighbors.
2. **Count Neighbors**: For each cell, count live neighbors using the board copy. This avoids using potentially changed neighbors on the original board.
3. **Apply Rules**: Use the count of live neighbors to decide the next state of the cell as per the rules:
   - A live cell with fewer than two or more than three live neighbors dies.
   - A dead cell with exactly three live neighbors becomes a live cell.

#### Time and Space Complexity

- **Time Complexity**: O(m * n), where m and n are the dimensions of the board, because we traverse each cell in the grid precisely once.
- **Space Complexity**: O(m * n) for the copy of the board.

```typescript
function gameOfLife(board: number[][]): void {
  const directions = [-1, 0, 1]; // direction vectors for neighboring cells
  const m = board.length;
  const n = board[0].length;

  // Create a copy of the board
  const copyBoard = board.map(row => [...row]);

  for (let row = 0; row < m; row++) {
    for (let col = 0; col < n; col++) {
      // Count live neighbors
      let liveNeighbors = 0;
      for (let i of directions) {
        for (let j of directions) {
          if (i === 0 && j === 0) continue; // skip the cell itself
          const r = row + i;
          const c = col + j;

          // Check if the neighbor is a valid cell and is live
          if (r >= 0 && r < m && c >= 0 && c < n && copyBoard[r][c] === 1) {
            liveNeighbors++;
          }
        }
      }

      // Apply Game of Life rules
      if (copyBoard[row][col] === 1 && (liveNeighbors < 2 || liveNeighbors > 3)) {
        board[row][col] = 0; // Cell dies
      }
      if (copyBoard[row][col] === 0 && liveNeighbors === 3) {
        board[row][col] = 1; // Cell becomes live
      }
    }
  }
}
```

### Approach 2: In-place Simulation with Encoding States

To optimize space usage, the simulation can be done in-place by encoding states using distinct integers beyond the given states (0 and 1). This approach leverages state transitions without needing a separate array.

#### Intuition

1. **In-place Encoding**: Encode transitions directly on the board:
   - Use `2` to encode a state change from live to dead.
   - Use `-1` to encode a state change from dead to live.
2. **Decoding Transition**: After updating all cells using encoded states, decode the board back to 0s and 1s.
3. **Same Neighbor Counting**: Count live neighbors using current values while ignoring encoded states.

#### Time and Space Complexity

- **Time Complexity**: O(m * n), similar as before since we iterate over each cell.
- **Space Complexity**: O(1), since no additional space besides the input board is used.

```typescript
function gameOfLife(board: number[][]): void {
  const directions = [-1, 0, 1];
  const m = board.length;
  const n = board[0].length;

  for (let row = 0; row < m; row++) {
    for (let col = 0; col < n; col++) {
      let liveNeighbors = 0;
      for (let i of directions) {
        for (let j of directions) {
          if (i === 0 && j === 0) continue;
          const r = row + i;
          const c = col + j;

          // Check if the neighbor is actual live, or encoded live (transition to dead)
          if (r >= 0 && r < m && c >= 0 && c < n && Math.abs(board[r][c]) === 1) {
            liveNeighbors++;
          }
        }
      }

      // Rule 1 or Rule 3: Live cell with <2 or >3 live neighbors dies
      if (board[row][col] === 1 && (liveNeighbors < 2 || liveNeighbors > 3)) {
        board[row][col] = 2; // Encode transition (1 -> 0)
      }
      // Rule 4: Dead cell with exactly three live neighbors becomes live
      if (board[row][col] === 0 && liveNeighbors === 3) {
        board[row][col] = -1; // Encode transition (0 -> 1)
      }
    }
  }

  // Final transition update
  for (let row = 0; row < m; row++) {
    for (let col = 0; col < n; col++) {
      if (board[row][col] === 2) {
        board[row][col] = 0; // Dead transition
      }
      if (board[row][col] === -1) {
        board[row][col] = 1; // Live transition
      }
    }
  }
}
```

These two approaches simulate the Game of Life efficiently, with the latter optimizing space by encoding states within the grid itself.

