# [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Approaches
- [Approach 1: Bellman-Ford Algorithm](#approach-1-bellman-ford-algorithm)
- [Approach 2: Dijkstra’s Algorithm Modified for K Stops](#approach-2-dijkstras-algorithm-modified-for-k-stops)
  
---

## Approach 1: Bellman-Ford Algorithm

### Intuition:
The Bellman-Ford algorithm is useful for finding the shortest paths in a graph with negative weights. For this problem, we can take advantage of Bellman-Ford's flexibility to implement a solution that allows us to be optimal within 'K' stop constraints.

### Steps:
1. Initialize distances from the source to all vertices as infinite (`Infinity`) except the source vertex which should be zero.
2. Repeat the following steps K+1 times:
   - For each edge `(u, v)` with cost `w`, if the merged distance from `src` to `v` via `u` is less than the current recorded distance to `v`, then update this distance.
3. Return the smallest distance to the destination, if unchanged, return -1 to signify no valid path was found within constraints.

### Code:
```javascript
function findCheapestPrice(n, flights, src, dst, K) {
    const dist = Array(n).fill(Infinity);
    dist[src] = 0;

    for (let i = 0; i <= K; i++) {
        const tempDist = dist.slice();

        for (const [u, v, w] of flights) {
            if (dist[u] === Infinity) continue; // do not continue from an unreachable node
            if (dist[u] + w < tempDist[v]) {
                tempDist[v] = dist[u] + w;
            }
        }

        dist = tempDist;
    }

    return dist[dst] === Infinity ? -1 : dist[dst];
}
```

### Complexity Analysis:
- **Time Complexity:** O(K * E), where E is the number of flights since we potentially iterate over each flight for each of the K+1 loops.
- **Space Complexity:** O(V), where V is the number of vertices to store the distance array.

---

## Approach 2: Dijkstra’s Algorithm Modified for K Stops

### Intuition:
This approach focuses on utilization of a priority queue to always extend the shortest (current) possible path to reach the destination within K stops. We modify Dijkstra by introducing a stop count limitation.

### Steps:
1. Create a graph representation of the flights.
2. Use a priority queue (min-heap) to extend the shortest paths first. Each element in the queue is a tuple of `(cost, node, stopsRemaining)`.
3. Begin from the `src` node. For the current element dequeued from the priority queue:
   - If it's the destination node, return the cost.
   - If there are stops remaining, attempt to relax the distances and add updated path costs back into the queue.
4. Continue until the destination is reached or no paths remain. Return -1 if the pathway is not found.

### Code:
```javascript
function findCheapestPrice(n, flights, src, dst, K) {
    const graph = new Map();

    flights.forEach(([u, v, w]) => {
        if (!graph.has(u)) graph.set(u, []);
        graph.get(u).push([v, w]);
    });

    const pq = [[0, src, K + 1]]; // priority queue initialized with [cost, node, stops]
    const minCost = Array(n).fill(Infinity);
    minCost[src] = 0;

    while (pq.length > 0) {
        const [cost, u, stops] = pq.shift();
        
        if (u === dst) return cost;
        if (stops > 0) {
            const neighbors = graph.get(u) || [];
            for (const [v, w] of neighbors) {
                if (cost + w < minCost[v]) {
                    minCost[v] = cost + w;
                    pq.push([cost + w, v, stops - 1]);
                }
            }
        }
    }

    return -1;
}
```

### Complexity Analysis:
- **Time Complexity:** O((V + E) * log(V)), for the priority queue operations.
- **Space Complexity:** O(V + E), for storing the graph representation.

This second approach is more optimal for larger datasets due to the efficient use of the priority queue but can become complex in terms of understanding and debugging.

