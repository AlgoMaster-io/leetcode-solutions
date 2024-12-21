# [Leetcode 289: Game of Life](https://leetcode.com/problems/game-of-life/)

## Approaches

- [Approach 1: Simulation using Temporary State](#approach-1-simulation-using-temporary-state)
- [Approach 2: Bit Manipulation](#approach-2-bit-manipulation)

### Approach 1: Simulation using Temporary State

#### Intuition

The Game of Life is a zero-player game that evolves based on its initial state. Each cell can either be live (1) or dead (0), and transitions happen simultaneously based on the states of the neighboring cells. A straightforward approach is to use a temporary state to record the changes. We can use a different number (like `2` for dead -> live and `-1` for live -> dead) to track the changes so that the original state isn't lost before the whole grid is updated.

The rules are:
1. Any live cell with fewer than two live neighbors dies.
2. Any live cell with two or three live neighbors lives on.
3. Any live cell with more than three live neighbors dies.
4. Any dead cell with exactly three live neighbors becomes a live cell.

#### Code

```go
func gameOfLife(board [][]int) {
    if len(board) == 0 || len(board[0]) == 0 {
        return
    }

    rows, cols := len(board), len(board[0])

    // Iterate through each cell
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            // Count alive neighbors
            liveNeighbors := countLiveNeighbors(board, r, c)

            // Apply rules:
            if board[r][c] == 1 {
                if liveNeighbors < 2 || liveNeighbors > 3 {
                    board[r][c] = -1 // mark cell to be dead
                }
            } else {
                if liveNeighbors == 3 {
                    board[r][c] = 2 // mark cell to be alive
                }
            }
        }
    }

    // Apply changes to the board
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if board[r][c] == -1 {
                board[r][c] = 0
            } else if board[r][c] == 2 {
                board[r][c] = 1
            }
        }
    }
}

func countLiveNeighbors(board [][]int, r int, c int) int {
    directions := []struct{ x, y int }{{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}}
    liveNeighbors := 0
    
    for _, d := range directions {
        nr, nc := r + d.x, c + d.y
        
        // Check bounds and count live or temporarily changed to dead
        if nr >= 0 && nr < len(board) && nc >= 0 && nc < len(board[0]) && (board[nr][nc] == 1 || board[nr][nc] == -1) {
            liveNeighbors++
        }
    }
    
    return liveNeighbors
}
```

#### Time and Space Complexity

- **Time complexity**: O(m*n), where m is the number of rows and n is the number of columns, because we have to visit each cell.
- **Space complexity**: O(1), as we are modifying the input board in place.

### Approach 2: Bit Manipulation

#### Intuition

Instead of using temporary states with arbitrary integer values, we can use bits to carry both current and next state information. We can use one bit to represent the current state and another bit to temporarily store the next state, thus not needing any extra space.

Here, we use:
- The least significant bit (LSB): To denote the current state.
- The second bit: To indicate the next state.

#### Code

```go
func gameOfLife(board [][]int) {
    if len(board) == 0 || len(board[0]) == 0 {
        return
    }

    rows, cols := len(board), len(board[0])

    // Iterate through each cell
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            liveNeighbors := countLiveNeighbors2(board, r, c)

            // Compute next state for board[r][c]
            if board[r][c] == 1 {
                if liveNeighbors == 2 || liveNeighbors == 3 {
                    board[r][c] |= 2 // Set second bit if live
                }
            } else {
                if liveNeighbors == 3 {
                    board[r][c] |= 2 // Set second bit if will become live
                }
            }
        }
    }

    // Update the board to the next state
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            board[r][c] >>= 1 // Right shift to get the next state
        }
    }
}

func countLiveNeighbors2(board [][]int, r int, c int) int {
    directions := []struct{ x, y int }{{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}}
    liveNeighbors := 0

    for _, d := range directions {
        nr, nc := r + d.x, c + d.y

        // Check bounds and count live current state
        if nr >= 0 && nr < len(board) && nc >= 0 && nc < len(board[0]) && (board[nr][nc] & 1) == 1 {
            liveNeighbors++
        }
    }

    return liveNeighbors
}
```

#### Time and Space Complexity

- **Time complexity**: O(m*n), where m is the number of rows and n is the number of columns, since we're visiting each cell.
- **Space complexity**: O(1), as we modify the board in place and use no additional data structures.

