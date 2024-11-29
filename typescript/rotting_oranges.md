# 994. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

## Approach: Breadth-First Search (BFS)

### Solution
typescript
```typescript
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the queue and visited cells

function orangesRotting(grid: number[][]): number {
    const rows = grid.length;
    const cols = grid[0].length;
    const queue: [number, number][] = [];

    let freshCount = 0;

    // Step 1: Initialize the queue with all rotten oranges and count fresh oranges
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (grid[i][j] === 2) {
                queue.push([i, j]);
            } else if (grid[i][j] === 1) {
                freshCount++;
            }
        }
    }

    // Step 2: Perform BFS to rot adjacent fresh oranges
    let minutes = 0;
    const directions: number[][] = [
        [0, 1], [1, 0], [0, -1], [-1, 0]
    ];

    while (queue.length > 0 && freshCount > 0) {
        const size = queue.length;
        for (let i = 0; i < size; i++) {
            const [currentRow, currentCol] = queue.shift()!;
            for (const [dx, dy] of directions) {
                const newRow = currentRow + dx;
                const newCol = currentCol + dy;

                // Check bounds and if the orange is fresh
                if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && grid[newRow][newCol] === 1) {
                    grid[newRow][newCol] = 2; // Rot the orange
                    queue.push([newRow, newCol]);
                    freshCount--;
                }
            }
        }
        minutes++;
    }

    // If there are still fresh oranges left, return -1
    return freshCount === 0 ? minutes : -1;
}
```

