# [Leetcode 1631: Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Approaches
- [Approach 1: Binary Search with Depth First Search (DFS)](#approach-1-binary-search-with-deep-first-search-dfs)
- [Approach 2: Dijkstra's Algorithm with Priority Queue](#approach-2-dijkstras-algorithm-with-priority-queue)

### Approach 1: Binary Search with Depth First Search (DFS)

**Intuition:**

The key idea here is to perform a Binary Search on the range of possible efforts and use Depth First Search (DFS) to verify if a certain effort threshold can allow us to travel from the top-left corner to the bottom-right corner of the grid. 

For each mid value (effort), we'll perform a DFS starting from the top-left corner. During the DFS, we only move to neighboring cells whose effort is within the current threshold (mid). If we reach the bottom-right corner with a certain mid, it means the path is feasible with that effort, and we might find a smaller effort in the lower half of the range. Otherwise, we need to try larger efforts, moving to the upper half.

**Steps:**
1. Define a helper function `canReachEnd` that performs DFS to check if the bottom-right corner is reachable with the current effort.
2. Initialize low and high bounds for binary search.
3. Perform binary search to find the minimum effort.

```javascript
function minimumEffortPath(heights) {
    const rows = heights.length;
    const cols = heights[0].length;
    
    const directions = [[0, 1], [0, -1], [1, 0], [-1, 0]];

    // Helper function to see if we can reach from (0, 0) to (rows-1, cols-1) with max effort of 'effort'
    function canReachEnd(effort) {
        const visited = Array.from({ length: rows }, () => Array(cols).fill(false));

        function dfs(x, y) {
            if (x === rows - 1 && y === cols - 1) return true;
            visited[x][y] = true;

            for (let [dx, dy] of directions) {
                const newX = x + dx;
                const newY = y + dy;

                if (newX >= 0 && newX < rows && newY >= 0 && newY < cols && !visited[newX][newY]) {
                    const edgeWeight = Math.abs(heights[newX][newY] - heights[x][y]);
                    if (edgeWeight <= effort) {
                        if (dfs(newX, newY)) return true;
                    }
                }
            }
            return false;
        }

        return dfs(0, 0);
    }
    
    let low = 0;
    let high = Math.max(...heights.map(row => Math.max(...row))) - Math.min(...heights.map(row => Math.min(...row)));
    let result = high;

    while (low <= high) {
        const mid = Math.floor((low + high) / 2);
        
        if (canReachEnd(mid)) {
            result = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }

    return result;
}
```

**Time Complexity:**  
- Binary search over the effort range: O(log(maxHeightDifference))
- DFS complexity for each binary search: O(V + E) where V is the number of vertices (cells) and E is the number of edges (4 directions * V).
- Overall complexity: O((V + E) * log(maxHeightDifference))

**Space Complexity:**  
- O(V) for the visited array, where V is the total number of cells.

### Approach 2: Dijkstra's Algorithm with Priority Queue

**Intuition:**

Use Dijkstra's algorithm to treat the grid as a graph where each cell is a node connected to its neighbors. The weight of the edge is the absolute difference in height. Use a priority queue to always expand the least effort path first, keeping track of the minimum effort required to reach each cell.

**Steps:**
1. Use a priority queue initialized with the top-left cell (with an effort of 0).
2. Pop the cell with the minimum effort from the queue and attempt to update its neighbors.
3. If a path to the bottom-right is found, since it's expanded in order of increasing effort, it will be the minimum.

```javascript
function minimumEffortPath(heights) {
    const rows = heights.length;
    const cols = heights[0].length;
    
    const directions = [[0, 1], [0, -1], [1, 0], [-1, 0]];
    const efforts = Array.from({ length: rows }, () => Array(cols).fill(Infinity));
    efforts[0][0] = 0;

    const pq = new MinPriorityQueue({ priority: (x) => x[0] });
    pq.enqueue([0, 0, 0]);  // [effort, x, y]

    while (!pq.isEmpty()) {
        const [currentEffort, x, y] = pq.dequeue().element;

        if (x === rows - 1 && y === cols - 1) return currentEffort;

        for (let [dx, dy] of directions) {
            const newX = x + dx;
            const newY = y + dy;

            if (newX >= 0 && newX < rows && newY >= 0 && newY < cols) {
                const newEffort = Math.max(currentEffort, Math.abs(heights[newX][newY] - heights[x][y]));

                if (newEffort < efforts[newX][newY]) {
                    efforts[newX][newY] = newEffort;
                    pq.enqueue([newEffort, newX, newY]);
                }
            }
        }
    }

    return -1;  // default return in case of no valid path, though problem guarantees one
}
```

**Time Complexity:**  
- O(V + E * log(V)), where V is the number of vertices (cells) and E is the number of edges, each operation with the priority queue takes log(V).

**Space Complexity:**  
- O(V) for the efforts matrix and the priority queue, as they store information for each cell.

The second approach is more optimal in practice as it ensures that we construct the minimum effort path using Dijkstra's algorithm which is specifically tailored for grid scenarios with edges having varying weights.

