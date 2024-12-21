# [Leetcode 289: Game of Life](https://leetcode.com/problems/game-of-life/)

## Approaches
- [Approach 1: Simulation with Extra Space](#approach-1-simulation-with-extra-space)
- [Approach 2: In-Place Solution with State Encoding](#approach-2-in-place-solution-with-state-encoding)

---

## Approach 1: Simulation with Extra Space

### Intuition
The Game of Life is a zero-player game that simulates the evolution of a grid of cells based on a set of rules. Each cell has 8 possible neighbors, and its state in the next generation depends on its current state and the number of live neighbors.

An easy way to solve this problem is to use an additional grid to store the next generation of states. We iterate through the original grid, compute the new state for each cell by considering its neighbors, and save the result in the new grid.

### Algorithm
1. Create a copy of the original board to store the next generation.
2. For each cell in the board:
    - Count the live neighbors.
    - Apply the rules of the Game of Life to determine the next state.
    - Update the next state in the copy board.
3. After processing all cells, copy the next generation from the copy board back to the original board.

### Time Complexity
- **O(M x N)**, where M is the number of rows and N is the number of columns in the board, because we iterate through each cell once.

### Space Complexity
- **O(M x N)** for storing the copy of the board.

```javascript
function gameOfLife(board) {
    const directions = [
        [0, 1], [1, 0], [0, -1], [-1, 0],
        [1, 1], [1, -1], [-1, 1], [-1, -1]
    ];
    
    const m = board.length;
    const n = board[0].length;
    
    const copyBoard = board.map(row => row.slice());
    
    for (let row = 0; row < m; row++) {
        for (let col = 0; col < n; col++) {
            // Count live neighbors
            let liveNeighbors = 0;
            for (const [dx, dy] of directions) {
                const newRow = row + dx;
                const newCol = col + dy;
                if (newRow >= 0 && newRow < m && newCol >= 0 && newCol < n && copyBoard[newRow][newCol] === 1) {
                    liveNeighbors += 1;
                }
            }
            
            // Apply rules
            if (copyBoard[row][col] === 1) {
                board[row][col] = liveNeighbors === 2 || liveNeighbors === 3 ? 1 : 0;
            } else {
                board[row][col] = liveNeighbors === 3 ? 1 : 0;
            }
        }
    }
}
```

---

## Approach 2: In-Place Solution with State Encoding

### Intuition
To overcome the space complexity of the previous approach, we can encode the state changes directly on the original board. The idea is to use two bits for each cell: the least significant bit for the current state and the second least significant bit for the next state. This way, changes are tracked in-place without needing another storage structure.

- '0' becomes '1' (2 in encoded form)
- '1' becomes '0' (3 in encoded form)

### Algorithm
1. For each cell, calculate the number of live neighbors.
2. Encode the next state into the cell using bit manipulation.
3. Update the board by shifting the bits.

### Time Complexity
- **O(M x N)**, similar to the previous approach.

### Space Complexity
- **O(1)**, since we don’t use additional space for another board.

```javascript
function gameOfLife(board) {
    const directions = [
        [0, 1], [1, 0], [0, -1], [-1, 0],
        [1, 1], [1, -1], [-1, 1], [-1, -1]
    ];

    const m = board.length;
    const n = board[0].length;

    for (let row = 0; row < m; row++) {
        for (let col = 0; col < n; col++) {
            // Count live neighbors
            let liveNeighbors = 0;
            for (const [dx, dy] of directions) {
                const newRow = row + dx;
                const newCol = col + dy;
                if (newRow >= 0 && newRow < m && newCol >= 0 && newCol < n && (board[newRow][newCol] & 1) === 1) {
                    liveNeighbors += 1;
                }
            }

            // Encode the next state
            if (board[row][col] === 1 && (liveNeighbors === 2 || liveNeighbors === 3)) {
                board[row][col] = 3; // 01 -> 11
            }
            if (board[row][col] === 0 && liveNeighbors === 3) {
                board[row][col] = 2; // 00 -> 10
            }
        }
    }

    // Shift bits to finalize the update
    for (let row = 0; row < m; row++) {
        for (let col = 0; col < n; col++) {
            board[row][col] >>= 1; // New state is shifted in
        }
    }
}
```

Both approaches solve the problem, but the second approach is more space-efficient, making it preferable for large inputs.

