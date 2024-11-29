# 1976. [Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

## Approach 1: Dijkstra's Algorithm with Modification

### Solution
```javascript
// Time Complexity: O(E log V) where E is the number of edges and V is the number of vertices
// Space Complexity: O(V)
function countPaths(n, roads) {
    const MOD = 1_000_000_007;
    const graph = Array.from({ length: n }, () => []);

    // Construct the graph
    for (const [u, v, time] of roads) {
        graph[u].push([v, time]);
        graph[v].push([u, time]);
    }

    // Dijkstra's algorithm modifications
    const minDist = Array(n).fill(Infinity);
    const ways = Array(n).fill(0);
    ways[0] = 1;
    minDist[0] = 0;

    const pq = new MinPriorityQueue({ priority: (item) => item[0] });
    pq.enqueue([0, 0]); // [distance, node]

    while (!pq.isEmpty()) {
        const [currentDist, u] = pq.dequeue().element;

        if (currentDist > minDist[u]) continue;

        // Traverse neighbors
        for (const [v, time] of graph[u]) {
            // Relaxation
            if (minDist[u] + time < minDist[v]) {
                minDist[v] = minDist[u] + time;
                ways[v] = ways[u];
                pq.enqueue([minDist[v], v]);
            } else if (minDist[u] + time === minDist[v]) {
                ways[v] = (ways[v] + ways[u]) % MOD;
            }
        }
    }

    return ways[n - 1];
}
```

This solution efficiently finds the number of shortest paths from node 0 to node n-1 using a modified version of Dijkstra's algorithm that keeps track of the number of ways to arrive at each node. It uses a priority queue to explore the shortest paths and maintains an array to store the number of ways to reach each node at the shortest distance.

