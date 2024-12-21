# [994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

## Approaches:
- [Breadth-First Search (BFS) Approach](#bfs-approach)
- [Multi-source BFS Optimized Approach](#multi-source-bfs-optimized-approach)

### BFS Approach

#### Intuition:
The problem of rotting oranges on a grid can be likened to a graph traversal problem. An orange becomes rotten in one minute if there is at least one rotten orange in its 4-directional neighborhood. This aligns well with a breadth-first search (BFS) strategy where each step represents the propagation of the rot from currently rotten oranges to their adjacent fresh oranges, incrementing the minute count with each level of BFS.

#### Detailed Steps:
1. **Initialize a Queue**: Use a queue to keep track of all the currently rotten oranges (level by level) and the minute at which they are processed.
2. **Traverse the Grid**: Enqueue all initial rotten oranges with time 0 and count all fresh oranges.
3. **BFS Processing**:
   - Dequeue an orange, and rot all its adjacent fresh oranges, decrementing the fresh orange count.
   - Enqueue these newly rotten oranges with the incremented time.
4. **Check Results**:
   - The last level processed gives the total time taken.
   - If there are still fresh oranges left, it implies they can't be rotten, hence return -1.

#### Complexity:
- **Time Complexity**: \(O(m \times n)\), where m is rows and n is columns, since each cell is processed once.
- **Space Complexity**: \(O(m \times n)\) in the worst case for the queue.

```javascript
function orangesRotting(grid) {
    const rows = grid.length;
    const cols = grid[0].length;
    let queue = [];
    let freshCount = 0;
    
    // Enqueuing all initially rotten oranges and counting fresh ones
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === 2) {
                queue.push([r, c, 0]); // enqueueing with time = 0
            } else if (grid[r][c] === 1) {
                freshCount++;
            }
        }
    }
    
    let timeTaken = 0;
    let directions = [[1, 0], [-1, 0], [0, 1], [0, -1]]; // possible 4 directions

    // BFS process
    while (queue.length > 0) {
        const [x, y, time] = queue.shift();
        timeTaken = time;

        for (const [dx, dy] of directions) {
            const nx = x + dx;
            const ny = y + dy;
            
            if (nx >= 0 && ny >= 0 && nx < rows && ny < cols && grid[nx][ny] === 1) {
                grid[nx][ny] = 2; // this fresh orange becomes rotten
                freshCount--; // decrement fresh orange counter
                queue.push([nx, ny, time + 1]); // enqueue with incremented time
            }
        }
    }

    return freshCount === 0 ? timeTaken : -1; // Check if all fresh oranges are rotten
}
```

### Multi-source BFS Optimized Approach

#### Intuition:
This approach is similar to the BFS Approach, but it uses the queue more efficiently by initially storing all the rotten oranges at once and processes them simultaneously. The optimization lies in reducing unnecessary computations by initially processing all rotten oranges which reduces the necessity to check for "fresh" oranges that are ultimately unreachable.

#### Detailed Steps:
1. **Identify Rotten Oranges**: Immediate identification and queueing of rotten oranges at a single instance.
2. **Propagate Rotting in a While Loop**: Simultaneous propagation using multi-source BFS ensures all oranges rot in the minimum time.
3. **Check Result**: As before, ensure if any fresh oranges remain.

#### Complexity:
- **Time Complexity**: \(O(m \times n)\), efficient propagation using multi-source BFS.
- **Space Complexity**: \(O(m \times n)\), for the queue storage of rotten oranges.

```javascript
function orangesRotting(grid) {
    const rows = grid.length;
    const cols = grid[0].length;
    let queue = [];
    let freshCount = 0;

    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === 2) {
                queue.push([r, c]); // add rotten oranges
            } else if (grid[r][c] === 1) {
                freshCount++; // count all fresh oranges
            }
        }
    }

    if (freshCount === 0) return 0; // No fresh oranges to rot

    let minutes = 0;
    let directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];
    
    while (queue.length && freshCount) {
        let size = queue.length;
        for (let i = 0; i < size; i++) {
            const [x, y] = queue.shift();
            
            for (const [dx, dy] of directions) {
                const nx = x + dx;
                const ny = y + dy;
                
                if (nx >= 0 && ny >= 0 && nx < rows && ny < cols && grid[nx][ny] === 1) {
                    grid[nx][ny] = 2; // Rot the orange
                    freshCount--; // one less fresh orange
                    queue.push([nx, ny]); // enqueue to process next
                }
            }
        }
        minutes++;
    }

    return freshCount ? -1 : minutes; // final check if there are fresh oranges left
}
```

These solutions utilize practical graph traversal techniques with a BFS style to solve the problem optimally, ensuring an efficient path toward a solution.

