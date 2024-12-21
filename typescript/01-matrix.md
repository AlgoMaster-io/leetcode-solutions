# [Leetcode 542: 01 Matrix](https://leetcode.com/problems/01-matrix/)

## Approaches
1. [Breadth-First Search (BFS) from Zero Cells](#bfs)
2. [Dynamic Programming (DP)](#dp)

---

### 1. Breadth-First Search (BFS) from Zero Cells

To solve this problem, we need to calculate the distance of each cell from the nearest 0. A straightforward approach is to perform a Breadth-First Search (BFS) starting from each zero cell. However, a direct application is inefficient, so here, we use multi-source BFS.

**Intuition:**  
Start the BFS from all zero cells simultaneously, and propagate the distance to neighbors. Essentially, we utilize the zero cells as starting points. Initially, mark all cells as infinity (or a very large number) except for zero cells. As the BFS progresses, update the distances of each cell based on their neighbors.

#### Steps:
1. Initialize a queue and enqueue all zero-valued cells.
2. Initialize a distance matrix with infinity for all non-zero cells.
3. Perform BFS, updating the neighbor cells' distances based on the current cell's distance plus one.
4. Return the updated distance matrix.

```typescript
function updateMatrix(mat: number[][]): number[][] {
    const m = mat.length;
    const n = mat[0].length;
    const directionVectors = [[0, 1], [1, 0], [0, -1], [-1, 0]];
    const distances = Array.from({ length: m }, () => Array(n).fill(Infinity));
    const queue: [number, number][] = [];

    // Initialize the queue with all 0-cells
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (mat[i][j] === 0) {
                distances[i][j] = 0;
                queue.push([i, j]);
            }
        }
    }

    // BFS
    while (queue.length > 0) {
        const [x, y] = queue.shift()!;
        for (const [dx, dy] of directionVectors) {
            const newX = x + dx;
            const newY = y + dy;
            if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                if (distances[newX][newY] > distances[x][y] + 1) {
                    distances[newX][newY] = distances[x][y] + 1;
                    queue.push([newX, newY]);
                }
            }
        }
    }

    return distances;
}
```

**Time Complexity:** \(O(m \cdot n)\) because each cell is processed once.  
**Space Complexity:** \(O(m \cdot n)\) because of the space needed to store the distances and queue.

---

### 2. Dynamic Programming (DP)

A more optimal and elegant solution can be achieved using Dynamic Programming. The idea is to perform two passes, one from the top-left to bottom-right, and one from the bottom-right to top-left.

**Intuition:**  
If you traverse the matrix from top-left to bottom-right, each cell gets the minimum distance from the top and left. Then, in the second pass, from bottom-right to top-left, each cell gets the minimum distance from the bottom and right. This ensures that every cell considers the minimum distance from all possible directions.

#### Steps:
1. Initialize the distance matrix with infinity for all non-zero cells and 0 for zero cells.
2. First pass: Update each cell from top-left to bottom-right using only the top and left distances.
3. Second pass: Update each cell from bottom-right to top-left using the bottom and right distances.
4. Return the updated distance matrix.

```typescript
function updateMatrixDP(mat: number[][]): number[][] {
    const m = mat.length;
    const n = mat[0].length;
    const distances = Array.from({ length: m }, () => Array(n).fill(Infinity));

    // First pass: check for top and left
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (mat[i][j] === 0) {
                distances[i][j] = 0;
            } else {
                if (i > 0) distances[i][j] = Math.min(distances[i][j], distances[i - 1][j] + 1);
                if (j > 0) distances[i][j] = Math.min(distances[i][j], distances[i][j - 1] + 1);
            }
        }
    }

    // Second pass: check for bottom and right
    for (let i = m - 1; i >= 0; i--) {
        for (let j = n - 1; j >= 0; j--) {
            if (i < m - 1) distances[i][j] = Math.min(distances[i][j], distances[i + 1][j] + 1);
            if (j < n - 1) distances[i][j] = Math.min(distances[i][j], distances[i][j + 1] + 1);
        }
    }

    return distances;
}
```

**Time Complexity:** \(O(m \cdot n)\) due to the two full matrix traversals.  
**Space Complexity:** \(O(1)\) additional space aside from the input and output storage, as we are modifying in place.

---

