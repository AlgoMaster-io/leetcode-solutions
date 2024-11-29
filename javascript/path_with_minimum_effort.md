# 1631. [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
```javascript
// Time Complexity: O(m * n * log(m * n)), where m is the number of rows and n is the number of columns.
// Space Complexity: O(m * n)
class Solution {
    // Direction vectors for exploring adjacent cells (right, left, down, up)
    static DIRECTIONS = [[0, 1], [1, 0], [0, -1], [-1, 0]];

    minimumEffortPath(heights) {
        const m = heights.length;
        const n = heights[0].length;

        // Min-heap priority queue to select the path with the minimum effort so far
        const minHeap = new MinPriorityQueue({ priority: (a) => a[2] });

        // Efforts matrix to track the minimum effort required to reach each cell
        const efforts = Array.from({ length: m }, () => Array(n).fill(Infinity));
        efforts[0][0] = 0;

        // Start from the top-left corner of the grid
        minHeap.enqueue([0, 0, 0]);

        while (!minHeap.isEmpty()) {
            // Extract the cell with the minimum effort
            const current = minHeap.dequeue().element;
            const [x, y, currentEffort] = current;

            // If we've reached the bottom-right corner
            if (x === m - 1 && y === n - 1) {
                return currentEffort;
            }

            // Explore all adjacent cells
            for (const direction of Solution.DIRECTIONS) {
                const [dx, dy] = direction;
                const newX = x + dx;
                const newY = y + dy;

                // Check bounds
                if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                    // Calculate the effort required to move to the adjacent cell
                    const effort = Math.max(currentEffort, Math.abs(heights[newX][newY] - heights[x][y]));

                    // If this path to the adjacent cell is better, update efforts and heap
                    if (effort < efforts[newX][newY]) {
                        efforts[newX][newY] = effort;
                        minHeap.enqueue([newX, newY, effort]);
                    }
                }
            }
        }

        return 0; // Shouldn't reach here. Always possible to reach the end.
    }
}
```

## Approach 2: Binary Search + Depth First Search (DFS)

### Solution
```javascript
// Time Complexity: O(m * n * log(maxDifference))
// Space Complexity: O(m * n)
class Solution {
    // Direction vectors for exploring adjacent cells (right, left, down, up)
    static DIRECTIONS = [[0, 1], [1, 0], [0, -1], [-1, 0]];

    minimumEffortPath(heights) {
        const m = heights.length;
        const n = heights[0].length;
        let left = 0, right = 1_000_000, result = 1_000_000;

        while (left <= right) {
            const mid = left + Math.floor((right - left) / 2);
            if (this.canReachEndWithEffort(heights, mid, m, n)) {
                result = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }

        return result;
    }

    // Helper function to determine if there is a path from (0, 0) to (m - 1, n - 1) under the given maxEffort
    canReachEndWithEffort(heights, maxEffort, m, n) {
        const visited = Array.from({ length: m }, () => Array(n).fill(false));
        const queue = [[0, 0]];
        visited[0][0] = true;

        while (queue.length > 0) {
            const [x, y] = queue.shift();

            if (x === m - 1 && y === n - 1) {
                return true;
            }

            for (const direction of Solution.DIRECTIONS) {
                const [dx, dy] = direction;
                const newX = x + dx;
                const newY = y + dy;

                if (newX >= 0 && newX < m && newY >= 0 && newY < n && !visited[newX][newY]) {
                    const currentEffort = Math.abs(heights[newX][newY] - heights[x][y]);

                    if (currentEffort <= maxEffort) {
                        visited[newX][newY] = true;
                        queue.push([newX, newY]);
                    }
                }
            }
        }

        return false;
    }
}
```


