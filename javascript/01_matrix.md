# 542. [01 Matrix](https://leetcode.com/problems/01-matrix/)

## Approach: Breadth-First Search (BFS)

### Solution

```javascript
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the queue and distance matrix

function updateMatrix(mat) {
    const rows = mat.length;
    const cols = mat[0].length;
    const distances = Array.from({ length: rows }, () => Array(cols).fill(0));
    const visited = Array.from({ length: rows }, () => Array(cols).fill(false));
    const queue = [];

    // Step 1: Initialize the queue with all '0' cells and mark them as visited
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (mat[i][j] === 0) {
                queue.push([i, j]);
                visited[i][j] = true;
            }
        }
    }

    // Step 2: Perform BFS to calculate distances
    const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];
    while (queue.length) {
        const [currentRow, currentCol] = queue.shift();
        for (const [dRow, dCol] of directions) {
            const newRow = currentRow + dRow;
            const newCol = currentCol + dCol;

            // Check bounds and if the cell is not visited
            if (
                newRow >= 0 &&
                newRow < rows &&
                newCol >= 0 &&
                newCol < cols &&
                !visited[newRow][newCol]
            ) {
                distances[newRow][newCol] = distances[currentRow][currentCol] + 1;
                queue.push([newRow, newCol]);
                visited[newRow][newCol] = true;
            }
        }
    }

    return distances;
}
```


