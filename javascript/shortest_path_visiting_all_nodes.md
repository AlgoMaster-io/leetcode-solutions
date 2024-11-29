# 847. [Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

## Approach 1: Breadth-First Search (BFS) with Bitmask

### Solution
```javascript
// Time Complexity: O(2^n * n^2)
// Space Complexity: O(2^n * n)

function shortestPathLength(graph) {
    const n = graph.length;
    if (n === 1) return 0;

    // Queue for BFS, storing [node, visited nodes mask]
    const queue = [];
    const visited = Array.from({ length: n }, () => Array(1 << n).fill(false));

    // Initialize queue and visited set with all starting nodes
    for (let i = 0; i < n; i++) {
        queue.push([i, 1 << i]);
        visited[i][1 << i] = true;
    }

    let steps = 0;
    while (queue.length) {
        const size = queue.length;
        for (let i = 0; i < size; i++) {
            const [node, mask] = queue.shift();

            // If all nodes are visited
            if (mask === (1 << n) - 1) return steps;

            // Visit all the neighbors
            for (const nei of graph[node]) {
                const nextMask = mask | (1 << nei);
                if (!visited[nei][nextMask]) {
                    queue.push([nei, nextMask]);
                    visited[nei][nextMask] = true;
                }
            }
        }
        steps++;
    }
    return -1;
}
```

## Approach 2: Dynamic Programming with Bitmask

### Solution
```javascript
// Time Complexity: O(2^n * n^2)
// Space Complexity: O(2^n * n)

function shortestPathLength(graph) {
    const n = graph.length;
    const dp = Array.from({ length: 1 << n }, () => Array(n).fill(Infinity));

    const queue = [];
    for (let i = 0; i < n; i++) {
        queue.push([1 << i, i]);
        dp[1 << i][i] = 0;
    }

    while (queue.length) {
        const [mask, node] = queue.shift();

        for (const nei of graph[node]) {
            const nextMask = mask | (1 << nei);
            if (dp[nextMask][nei] > dp[mask][node] + 1) {
                dp[nextMask][nei] = dp[mask][node] + 1;
                queue.push([nextMask, nei]);
            }
        }
    }

    let result = Infinity;
    for (let i = 0; i < n; i++) {
        result = Math.min(result, dp[(1 << n) - 1][i]);
    }
    return result;
}
```

Both approaches use BFS and dynamic programming with bitmask to efficiently find the shortest path to visit all nodes, ensuring optimality in solving the problem.

