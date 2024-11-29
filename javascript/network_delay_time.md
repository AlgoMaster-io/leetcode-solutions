# 743. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
```javascript
// Time Complexity: O((V + E) log V)
// Space Complexity: O(V + E)
function networkDelayTime(times, n, k) {
    // Create a graph as adjacency list representation
    const graph = new Map();
    for (const [u, v, w] of times) {
        if (!graph.has(u)) {
            graph.set(u, []);
        }
        graph.get(u).push([v, w]);
    }

    // Priority Queue to hold nodes as pairs of (time, node)
    const pq = new MinPriorityQueue({ priority: ([time]) => time });
    pq.enqueue([0, k]);

    // Map to track the minimum time to reach each node
    const minTimeToReach = new Map();

    // Dijkstra's algorithm
    while (!pq.isEmpty()) {
        const [currentTime, currentNode] = pq.dequeue().element;

        if (minTimeToReach.has(currentNode)) 
            continue;

        minTimeToReach.set(currentNode, currentTime);
        if (graph.has(currentNode)) {
            for (const [nextNode, travelTime] of graph.get(currentNode)) {
                if (!minTimeToReach.has(nextNode)) {
                    pq.enqueue([currentTime + travelTime, nextNode]);
                }
            }
        }
    }

    // Determine the maximum time needed
    if (minTimeToReach.size !== n) 
        return -1;

    let maxTime = 0;
    for (const time of minTimeToReach.values()) {
        maxTime = Math.max(maxTime, time);
    }

    return maxTime;
}
```

## Approach 2: Bellman-Ford Algorithm

### Solution
```javascript
// Time Complexity: O(V * E)
// Space Complexity: O(V)
function networkDelayTime(times, n, k) {
    const dist = Array(n + 1).fill(Infinity);
    dist[k] = 0;

    // Relaxation process for V-1 times
    for (let i = 1; i < n; i++) {
        for (const [u, v, w] of times) {
            if (dist[u] !== Infinity && dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
            }
        }
    }

    let maxTime = 0;
    for (let i = 1; i <= n; i++) {
        if (dist[i] === Infinity) 
            return -1;
        maxTime = Math.max(maxTime, dist[i]);
    }

    return maxTime;
}
```

## Approach 3: Dynamic Programming with Floyd-Warshall Algorithm

### Solution
```javascript
// Time Complexity: O(V^3)
// Space Complexity: O(V^2)
function networkDelayTime(times, n, k) {
    const dp = Array.from({ length: n + 1 }, () => Array(n + 1).fill(Infinity));
    for (let i = 0; i <= n; i++) {
        dp[i][i] = 0; // Distance from a node to itself is 0
    }

    // Initialize graph with given times
    for (const [u, v, w] of times) {
        dp[u][v] = w;
    }

    // Floyd-Warshall algorithm for shortest paths
    for (let t = 1; t <= n; t++) {
        for (let u = 1; u <= n; u++) {
            for (let v = 1; v <= n; v++) {
                if (dp[u][t] !== Infinity && dp[t][v] !== Infinity) {
                    dp[u][v] = Math.min(dp[u][v], dp[u][t] + dp[t][v]);
                }
            }
        }
    }

    // Calculate the longest shortest path from the start node k
    let maxTime = 0;
    for (let i = 1; i <= n; i++) {
        if (dp[k][i] === Infinity) 
            return -1;
        maxTime = Math.max(maxTime, dp[k][i]);
    }

    return maxTime;
}
```

