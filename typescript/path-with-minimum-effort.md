# [Leetcode 1631: Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Approaches
1. [Basic Breadth-First Search Approach](#basic-breadth-first-search-approach)
2. [Dijkstra Inspired Approach Using Min-Heap](#dijkstra-inspired-approach-using-min-heap)

---

### Basic Breadth-First Search Approach

#### Intuition
The problem can be visualized as finding a path in a graph, with nodes representing each cell in the grid and edges representing the absolute differences between adjacent cells, aiming to minimize the maximum edge weight along the path from the top-left to the bottom-right corner. A straightforward approach is to perform a breadth-first search (BFS), keeping track of the maximum effort encountered along the path.

#### Explanation
1. We utilize a BFS traversal starting from the top-left corner of the grid.
2. A 2D array `maxEffort` is used to store the maximum effort required to reach each cell. Every cell is initially set to infinity except the starting cell.
3. We maintain a queue for the BFS traversal and process each cell by visiting its unvisited neighbors.
4. For each neighbor, update its `maxEffort` if the path to it has a lower maximum effort than previously recorded.
5. Continue this until we reach the bottom-right corner.

#### Time and Space Complexity
- **Time Complexity**: O(M * N * log(M * N)) - The BFS traversal visits each cell, and the log factor comes from updating the priority queue.
- **Space Complexity**: O(M * N) - The space required for the maxEffort array and the queue.

#### Code
```typescript
type Cell = [number, number, number];

function minimumEffortPath(heights: number[][]): number {
    const rows = heights.length;
    const cols = heights[0].length;
    const directions = [[0,1], [1,0], [-1,0], [0,-1]];
    const maxEffort = Array.from({ length: rows }, () => Array(cols).fill(Infinity));
    const queue: Cell[] = [[0, 0, 0]];

    maxEffort[0][0] = 0;

    while (queue.length > 0) {
        const [currEffort, row, col] = queue.shift()!;
        
        for (const [dr, dc] of directions) {
            const newRow = row + dr;
            const newCol = col + dc;
            if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols) {
                const effort = Math.max(currEffort, Math.abs(heights[newRow][newCol] - heights[row][col]));
                if (effort < maxEffort[newRow][newCol]) {
                    maxEffort[newRow][newCol] = effort;
                    queue.push([effort, newRow, newCol]);
                }
            }
        }
    }

    return maxEffort[rows-1][cols-1];
}
```

---

### Dijkstra Inspired Approach Using Min-Heap

#### Intuition
The BFS approach doesn't efficiently handle edge costs optimally. Instead, a more optimal way is to treat the problem similarly to Dijkstra's algorithm where we start at the top-left and use a priority queue to always expand the path with the minimum "effort" first. This approach efficiently finds the path due to the priority mechanism of the heap.

#### Explanation
1. We still use the 2D `maxEffort` array but now utilize a min-heap to prioritize paths with the minimum effort.
2. Initialize the heap with the starting cell.
3. While the heap is not empty, extract the cell with the smallest effort.
4. Traverse its neighbors and update the path's maximum effort.
5. If reaching the bottom-right corner, return the effort immediately as it's the minimum by nature of the heap.

#### Time and Space Complexity
- **Time Complexity**: O(M * N * log(M * N)) - Min-Heap operations dominate due to insertion and extraction.
- **Space Complexity**: O(M * N) - Due to the data structures used for storage and tracking paths.

#### Code
```typescript
type CellHeap = [number, number, number]; // [effort, row, column]

function minimumEffortPathDijkstra(heights: number[][]): number {
    const rows = heights.length;
    const cols = heights[0].length;
    const directions = [[0, 1], [1, 0], [-1, 0], [0, -1]];
    const maxEffort = Array.from({ length: rows }, () => Array(cols).fill(Infinity));

    const minHeap: CellHeap[] = [];
    minHeap.push([0, 0, 0]); // Starting point, zero effort
    maxEffort[0][0] = 0;

    const compareCells = (a: CellHeap, b: CellHeap) => a[0] - b[0];

    while (minHeap.length > 0) {
        minHeap.sort(compareCells);
        const [currEffort, row, col] = minHeap.shift()!;

        if (row === rows - 1 && col === cols - 1) {
            return currEffort;
        }

        for (const [dr, dc] of directions) {
            const newRow = row + dr;
            const newCol = col + dc;
            if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols) {
                const effort = Math.max(currEffort, Math.abs(heights[newRow][newCol] - heights[row][col]));
                if (effort < maxEffort[newRow][newCol]) {
                    maxEffort[newRow][newCol] = effort;
                    minHeap.push([effort, newRow, newCol]);
                }
            }
        }
    }

    return -1; // theoretically unreachable, since we should process the last cell within the loop and return
}
```

Both approaches provide a solution to find the path with the minimum maximum effort from the top-left to the bottom-right corner of the grid in different ways, each with its complexity and method of handling the path evaluation.

