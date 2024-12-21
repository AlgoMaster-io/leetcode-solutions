# [01 Matrix](https://leetcode.com/problems/01-matrix/)

## Approaches:
- [Approach 1: Breadth-First Search (BFS) from Zeros](#approach-1)
- [Approach 2: Dynamic Programming](#approach-2)

### Approach 1: Breadth-First Search (BFS) from Zeros

#### Intuition:
The main idea here is to use the breadth-first search approach to calculate the shortest distance from each cell to its closest zero. We first enqueue all positions with 0 values and then process each layer in BFS manner. For each cell, we record the shortest distance to a 0, increasing the distance level by level.

#### Algorithm:
1. Initialize an empty queue for BFS and a distance matrix filled with Infinity (except for cells with 0, which are initialized with a distance of 0).
2. Enqueue all positions containing `0` in the matrix.
3. Perform BFS:
   - Dequeue an element and explore its four neighbors (up, down, left, right).
   - If the neighbor has a larger distance value, update it to the current cell's distance + 1 and enqueue the neighbor.
4. Continue this process until the queue is empty.

```javascript
function updateMatrix(mat) {
    const rows = mat.length;
    const cols = mat[0].length;
    const dist = Array.from({ length: rows }, () => Array(cols).fill(Infinity));
    const queue = [];
    
    // Enqueue all cells with 0 and set their distance to 0
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (mat[r][c] === 0) {
                dist[r][c] = 0;
                queue.push([r, c]);
            }
        }
    }

    const directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];

    while (queue.length) {
        const [r, c] = queue.shift();

        // Explore all 4 directions
        for (let [dr, dc] of directions) {
            const nr = r + dr;
            const nc = c + dc;

            // If the neighbor is valid and the distance can be minimized, update it
            if (nr >= 0 && nr < rows && nc >= 0 && nc < cols && dist[nr][nc] > dist[r][c] + 1) {
                dist[nr][nc] = dist[r][c] + 1;
                queue.push([nr, nc]);
            }
        }
    }

    return dist;
}
```

- **Time Complexity**: O(m * n), where m is the number of rows and n is the number of columns. Each cell is processed once.
- **Space Complexity:** O(m * n) for the queue in the worst case.

### Approach 2: Dynamic Programming

#### Intuition:
This approach uses the idea of dynamic programming based on the update sequence of matrix cells. We can break the task into two main passes over the matrix: one from top-left to bottom-right and another from bottom-right to top-left. In each pass, update the cell values considering the minimum possible distances from their neighbors.

#### Algorithm:
1. Initialize the distance matrix with a large number for `1`s and zero for `0`s.
2. Traverse the matrix from top-left to bottom-right:
   - For each cell, check only the top and left neighbors and update the current cell.
3. Traverse the matrix from bottom-right to top-left:
   - Check bottom and right neighbors to update the current cell.
4. After these two passes, each cell in the matrix has the minimal distance to a zero.

```javascript
function updateMatrix(mat) {
    const rows = mat.length;
    const cols = mat[0].length;
    const maxDistance = rows + cols;
    const dist = Array.from({ length: rows }, () => Array(cols).fill(maxDistance));

    // Update with top and left neighbors
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (mat[r][c] === 0) {
                dist[r][c] = 0;
            } else {
                if (r > 0) {
                    dist[r][c] = Math.min(dist[r][c], dist[r - 1][c] + 1);
                }
                if (c > 0) {
                    dist[r][c] = Math.min(dist[r][c], dist[r][c - 1] + 1);
                }
            }
        }
    }

    // Update with bottom and right neighbors
    for (let r = rows - 1; r >= 0; r--) {
        for (let c = cols - 1; c >= 0; c--) {
            if (r < rows - 1) {
                dist[r][c] = Math.min(dist[r][c], dist[r + 1][c] + 1);
            }
            if (c < cols - 1) {
                dist[r][c] = Math.min(dist[r][c], dist[r][c + 1] + 1);
            }
        }
    }

    return dist;
}
```

- **Time Complexity**: O(m * n). We pass through each cell two times (in two passes).
- **Space Complexity**: O(1) additional space, ignoring the space used by the output matrix.

