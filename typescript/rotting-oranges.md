# [Leetcode 994: Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

## Approaches
1. [Breadth-First Search (BFS) Approach](#bfs-approach)

---

## BFS Approach

### Intuition
The problem can be boiled down to computing the "minimum time to propagate an effect across neighbors", which is akin to finding the shortest path in an unweighted graph. This can be efficiently solved with a Breadth-First Search (BFS) for level-order traversal, where each level represents a time unit.

The grid can be visualized as a graph where each orange at position `(i, j)` can spread rot to its four neighbors: `(i-1, j)`, `(i+1, j)`, `(i, j-1)`, `(i, j+1)`. 

We start by enqueuing all initially rotten oranges and proceed to enqueue freshly rotten oranges at each time unit until no fresh orange can rot further.

### Detailed Steps
1. Traverse the grid to find all initially rotten oranges and count the fresh oranges. Add the rotten oranges to a queue to process them level-wise.
2. Use a BFS approach by processing all rotten oranges in the queue - for each orange, mark its adjacent fresh oranges (if any) as rotten and add them to the queue.
3. Keep track of the time elapsed and decrement the count of fresh oranges as they rot.
4. The process stops when there are no more rotten oranges to process or no fresh oranges left.
5. If there are fresh oranges left that cannot rot due to isolation, return `-1`. Otherwise, return the total time elapsed.

### Code
```typescript
function orangesRotting(grid: number[][]): number {
    const rows = grid.length;
    const cols = grid[0].length;
    let freshCount = 0;
    const queue: [number, number][] = [];

    // Initialize the queue with all rotten oranges' positions and count fresh oranges
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === 2) {
                queue.push([r, c]);
            }
            if (grid[r][c] === 1) {
                freshCount++;
            }
        }
    }

    // Directions for moving up, down, left, right
    const directions = [
        [-1, 0], [1, 0],
        [0, -1], [0, 1]
    ];

    let timeElapsed = 0;

    // Process the queue using BFS
    while (queue.length > 0 && freshCount > 0) {
        const size = queue.length;
        for (let i = 0; i < size; i++) {
            const [x, y] = queue.shift()!;

            for (let [dx, dy] of directions) {
                const newX = x + dx;
                const newY = y + dy;
                
                // If the adjacent orange is fresh, it will rot
                if (
                    newX >= 0 && newY >= 0 &&
                    newX < rows && newY < cols &&
                    grid[newX][newY] === 1
                ) {
                    grid[newX][newY] = 2; // Mark the fresh orange as rotten
                    freshCount--; // Decrease the count of fresh oranges
                    queue.push([newX, newY]); // Enqueue the newly rotten orange
                }
            }
        }
        timeElapsed++; // Increment the time after processing one layer
    }

    // If there are any fresh oranges left, return -1
    return freshCount === 0 ? timeElapsed : -1;
}
```

### Complexity Analysis
- **Time Complexity**: O(R \* C), where R is the number of rows and C is the number of columns in the grid. We may potentially visit each cell once.
- **Space Complexity**: O(R \* C) for the queue in the worst case when all cells are in the queue.

