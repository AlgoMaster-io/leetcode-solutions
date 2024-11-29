# 743. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
typescript
```typescript
// Time Complexity: O((V + E) log V)
// Space Complexity: O(V + E)
function networkDelayTime(times: number[][], n: number, k: number): number {
    // Create a graph as adjacency list representation
    const graph: Map<number, number[][]> = new Map();
    for (const time of times) {
        if (!graph.has(time[0])) {
            graph.set(time[0], []);
        }
        graph.get(time[0])!.push([time[1], time[2]]);
    }

    // Priority Queue to hold nodes as pairs of (time, node)
    const pq: [number, number][] = [];
    pq.push([0, k]);

    // Map to track the minimum time to reach each node
    const minTimeToReach: Map<number, number> = new Map();

    // Dijkstra's algorithm
    while (pq.length > 0) {
        pq.sort((a, b) => a[0] - b[0]);
        const [currentTime, currentNode] = pq.shift()!;

        if (minTimeToReach.has(currentNode)) 
            continue;

        minTimeToReach.set(currentNode, currentTime);
        if (graph.has(currentNode)) {
            for (const adj of graph.get(currentNode)!) {
                const [nextNode, travelTime] = adj;
                if (!minTimeToReach.has(nextNode)) {
                    pq.push([currentTime + travelTime, nextNode]);
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
typescript
```typescript
// Time Complexity: O(V * E)
// Space Complexity: O(V)
function networkDelayTimeBellmanFord(times: number[][], n: number, k: number): number {
    const dist: number[] = new Array(n + 1).fill(Infinity);
    dist[k] = 0;

    // Relaxation process for V-1 times
    for (let i = 1; i < n; i++) {
        for (const edge of times) {
            const [u, v, w] = edge;
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
typescript
```typescript
// Time Complexity: O(V^3)
// Space Complexity: O(V^2)
function networkDelayTimeFloydWarshall(times: number[][], n: number, k: number): number {
    const dp: number[][] = new Array(n + 1).fill(0).map(() => new Array(n + 1).fill(Infinity / 2));
    for (let i = 0; i <= n; i++) {
        dp[i][i] = 0; // Distance from a node to itself is 0
    }

    // Initialize graph with given times
    for (const time of times) {
        dp[time[0]][time[1]] = time[2];
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


