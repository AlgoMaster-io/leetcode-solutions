# [Leetcode 130: Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

## Approaches
- [Depth-First Search (DFS)](#depth-first-search-dfs)
- [Breadth-First Search (BFS)](#breadth-first-search-bfs)

### Depth-First Search (DFS)

#### Intuition
The problem requires us to capture surrounded regions of 'O' by flipping them to 'X'. An 'O' region is surrounded if it is not connected to any 'O' on the boundary (first or last row/column). Hence, the key to solving this problem is identifying the 'O' regions that are connected to the border and thus should not be flipped.

The idea is to start from the boundary and perform a DFS to mark all 'O's connected to the boundary. These marked 'O's won't be turned to 'X'. After marking, we iterate through the board and flip all unmarked 'O's to 'X'.

#### Implementation
```python
def solve(board):
    if not board:
        return
    rows, cols = len(board), len(board[0])
    
    def dfs(r, c):
        if r < 0 or c < 0 or r >= rows or c >= cols or board[r][c] != 'O':
            return
        # Mark the cell as visited by changing it to 'E'
        board[r][c] = 'E'
        dfs(r-1, c)
        dfs(r+1, c)
        dfs(r, c-1)
        dfs(r, c+1)
    
    # Check for 'O's on the boundary and perform DFS
    for r in range(rows):
        for c in [0, cols-1]:
            dfs(r, c)
    for r in [0, rows-1]:
        for c in range(cols):
            dfs(r, c)
    
    for r in range(rows):
        for c in range(cols):
            if board[r][c] == 'E':
                # Restore 'E' back to 'O'
                board[r][c] = 'O'
            elif board[r][c] == 'O':
                # Flip surrounded 'O' to 'X'
                board[r][c] = 'X'

```

#### Time Complexity
- **Time**: \(O(m \times n)\) where \(m\) is the number of rows and \(n\) is the number of columns.
- **Space**: \(O(m \times n)\) for the recursion stack space.

### Breadth-First Search (BFS)

#### Intuition
Similar to DFS, we want to mark the non-surrounded 'O's starting from the boundary using a BFS approach. Instead of using recursive calls, we use a queue to iteratively explore the neighbors of each 'O' connected to the boundary.

#### Implementation
```python
from collections import deque

def solve(board):
    if not board:
        return
    rows, cols = len(board), len(board[0])
    queue = deque()
    
    # Helper function to enqueue boundary 'O's
    def enqueue(r, c):
        if 0 <= r < rows and 0 <= c < cols and board[r][c] == 'O':
            # Mark the cell as visited by changing it to 'E'
            board[r][c] = 'E'
            queue.append((r, c))
    
    # Check for 'O's on the boundary and initialize the queue
    for r in range(rows):
        for c in [0, cols-1]:
            enqueue(r, c)
    for r in [0, rows-1]:
        for c in range(cols):
            enqueue(r, c)
    
    # Perform BFS from the boundary 'O's
    while queue:
        r, c = queue.popleft()
        for dr, dc in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
            enqueue(r + dr, c + dc)
    
    # Flip surrounded 'O' to 'X' and restore non-surrounded 'O'
    for r in range(rows):
        for c in range(cols):
            if board[r][c] == 'E':
                board[r][c] = 'O'
            elif board[r][c] == 'O':
                board[r][c] = 'X'

```

#### Time Complexity
- **Time**: \(O(m \times n)\) where \(m\) is the number of rows and \(n\) is the number of columns.
- **Space**: \(O(\min(m, n))\) for the queue, as the queue can potentially grow to this size during BFS.

