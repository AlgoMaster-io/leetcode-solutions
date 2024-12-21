# [Leetcode 1976: Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination)

## Approaches

1. [Dijkstra's Algorithm with Basic Path Counting](#approach-1-dijkstras-algorithm-with-basic-path-counting)
2. [Optimized Dijkstra's Algorithm using Priority Queue](#approach-2-optimized-dijkstras-algorithm-using-priority-queue)

---

## Approach 1: Dijkstra's Algorithm with Basic Path Counting

### Intuition

To solve the problem of finding the number of ways to reach the destination with the minimum cost, we can use Dijkstra's algorithm. Dijkstra's algorithm is useful because it finds the shortest path from a starting node to all other nodes in a weighted graph. By integrating a path counting mechanism, we can count the number of shortest paths to each node.

We will maintain:

- `dist[i]`: Minimum cost to reach node `i`.
- `ways[i]`: Number of ways to reach node `i` with a cost of `dist[i]`.

### Steps

1. Initialize `dist` array with infinity for each node except the source, which is initialized with 0.
2. Set `ways[source]` to 1 since there's exactly one way to be at the source initially.
3. Use a min-heap to keep track of the next node with the shortest known distance.
4. For each node processed from the heap, update its neighbors:
   - If a shorter path is found, update the distance and reset the count of ways.
   - If another path with the same minimum cost is found, add to the existing count of ways.
5. Continue until all nodes are processed.

### Code

```javascript
const countPaths = function(n, roads) {
    const MOD = 1e9 + 7;

    // Construct the adjacency list
    const adj = Array.from({ length: n }, () => []);
    for (const [u, v, w] of roads) {
        adj[u].push([v, w]);
        adj[v].push([u, w]);
    }

    // Min-heap for Dijkstra's algorithm
    const heap = [[0, 0]]; // [distance, node]
    const dist = Array(n).fill(Infinity);
    const ways = Array(n).fill(0);
    dist[0] = 0;
    ways[0] = 1;

    while (heap.length > 0) {
        heap.sort((a, b) => a[0] - b[0]); // Sort heap for minimum distance
        const [currentDist, node] = heap.shift(); // Get the node with the shortest distance

        for (const [nei, roadCost] of adj[node]) {
            const newDist = currentDist + roadCost;

            if (newDist < dist[nei]) {
                dist[nei] = newDist;
                ways[nei] = ways[node];
                heap.push([newDist, nei]);
            } else if (newDist === dist[nei]) {
                ways[nei] = (ways[nei] + ways[node]) % MOD;
            }
        }
    }

    return ways[n - 1];
};
```

### Time and Space Complexity

- **Time Complexity**: \(O((V + E) \log V)\), where \(V\) is the number of nodes and \(E\) is the number of edges. This arises from each insertion and extraction operation on the priority queue.
- **Space Complexity**: \(O(V + E)\), for the adjacency list, distance array, and ways array.

---

## Approach 2: Optimized Dijkstra's Algorithm using Priority Queue

### Intuition

To further optimize, we can use a priority queue (or min-heap) for more efficient extraction of the minimum distance node, guaranteeing logarithmic time complexity for each operation. This ensures a faster processing of nodes as compared to sorting the heap from the previous approach.

### Steps

1. Use a priority queue instead of sorting the heap to get better performance.
2. The rest of the logic remains the same.

### Code

```javascript
const countPathsOptimized = function(n, roads) {
    const MOD = 1e9 + 7;

    // Construct the adjacency list
    const adj = Array.from({ length: n }, () => []);
    for (const [u, v, w] of roads) {
        adj[u].push([v, w]);
        adj[v].push([u, w]);
    }

    // Priority queue for Dijkstra's algorithm
    const PriorityQueue = require('collections/heap'); // Can use any priority queue implementation
    const heap = new PriorityQueue([], null, (a, b) => b[0] - a[0]);
    heap.push([0, 0]); // [distance, node]

    const dist = Array(n).fill(Infinity);
    const ways = Array(n).fill(0);
    dist[0] = 0;
    ways[0] = 1;

    while (heap.length > 0) {
        const [currentDist, node] = heap.pop(); // Get the node with the shortest distance

        for (const [nei, roadCost] of adj[node]) {
            const newDist = currentDist + roadCost;

            if (newDist < dist[nei]) {
                dist[nei] = newDist;
                ways[nei] = ways[node];
                heap.push([newDist, nei]);
            } else if (newDist === dist[nei]) {
                ways[nei] = (ways[nei] + ways[node]) % MOD;
            }
        }
    }

    return ways[n - 1];
};
```

### Time and Space Complexity

- **Time Complexity**: \(O((V + E) \log V)\), leveraging the priority queue for efficient operations.
- **Space Complexity**: \(O(V + E)\) due to the adjacency list, distance, and ways arrays.

