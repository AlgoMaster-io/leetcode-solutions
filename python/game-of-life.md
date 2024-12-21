# [Leetcode 289: Game of Life](https://leetcode.com/problems/game-of-life/)

## Approaches
1. [Brute Force](#approach-1-brute-force)
2. [In-Place with State Encoding](#approach-2-in-place-with-state-encoding)

### Approach 1: Brute Force

**Intuition:**

The basic idea of the "Game of Life" is to count the number of live neighbors for each cell and then determine whether the cell will be alive or dead in the next state based on the given rules. The brute force method implements this by creating a new board and updating it based on the state of the original board.

**Steps:**

1. Create a copy of the current board, which we can update while iterating over the original board.
2. For each cell, count the live neighbors by examining all 8 possible directions.
3. Update the cell in the new board based on the number of live neighbors and the current state of the cell.
4. Copy the updated values back to the original board after processing all cells.

**Code:**
```python
def gameOfLife(board):
    # Create a deep copy of the board to store guesses.
    def copy_board(board):
        return [[board[r][c] for c in range(len(board[0]))] for r in range(len(board))]
    
    # Calculate live neighbors for a given cell.
    def live_neighbors(board, row, col):
        directions = [(-1, -1), (-1, 0), (-1, 1), 
                      ( 0, -1)        , ( 0, 1), 
                      ( 1, -1), ( 1, 0), ( 1, 1)]
        
        count = 0
        rows, cols = len(board), len(board[0])
        for dr, dc in directions:
            r, c = row + dr, col + dc
            if 0 <= r < rows and 0 <= c < cols and board[r][c] == 1:
                count += 1
        return count
    
    # Rule implementation    
    original = copy_board(board)
    for r in range(len(board)):
        for c in range(len(board[0])):
            live_count = live_neighbors(original, r, c)
            
            # Rule 1 or 3
            if original[r][c] == 1 and (live_count < 2 or live_count > 3):
                board[r][c] = 0
            # Rule 4
            elif original[r][c] == 0 and live_count == 3:
                board[r][c] = 1

# Time complexity: O(m*n) for iterating over each cell and its neighbors.
# Space complexity: O(m*n) for the copy of the board.
```

### Approach 2: In-Place with State Encoding

**Intuition:**

Instead of using a separate board to store the next state, we can encode the state change within the current board itself by using intermediate values. This allows us to work in-place and reduces the space complexity.

- Use `2` to represent a cell that is currently alive but will die.
- Use `3` to represent a cell that is currently dead but will become alive.

**Steps:**

1. For each cell, count live neighbors as in the brute force approach.
2. Use the encoding values `2` and `3` to reflect state transitions in-place.
3. Once all cells are processed, update the board to reflect the final state by interpreting the encoded values.

**Code:**
```python
def gameOfLife(board):
    def live_neighbors(row, col):
        directions = [(-1, -1), (-1, 0), (-1, 1),
                      ( 0, -1)        , ( 0, 1),
                      ( 1, -1), ( 1, 0), ( 1, 1)]
        
        count = 0
        rows, cols = len(board), len(board[0])
        for dr, dc in directions:
            r, c = row + dr, col + dc
            if 0 <= r < rows and 0 <= c < cols and abs(board[r][c]) == 1:
                count += 1
        return count
    
    for r in range(len(board)):
        for c in range(len(board[0])):
            live_count = live_neighbors(r, c)
            
            # Alive cell with fewer than 2 or more than 3 neighbors dies.
            if board[r][c] == 1 and (live_count < 2 or live_count > 3):
                board[r][c] = 2
            
            # Dead cell with exactly 3 neighbors becomes alive.
            if board[r][c] == 0 and live_count == 3:
                board[r][c] = 3
    
    for r in range(len(board)):
        for c in range(len(board[0])):
            if board[r][c] == 2:
                board[r][c] = 0  # Die to dead
            if board[r][c] == 3:
                board[r][c] = 1  # Dead to alive

# Time complexity: O(m*n) for checking each cell and its neighbors.
# Space complexity: O(1) since we are modifying the board in-place.
```

These approaches show a progression from a basic implementation with additional space usage to a more optimal in-place solution.

