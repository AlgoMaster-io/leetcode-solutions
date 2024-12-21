# [Leetcode 743: Network Delay Time](https://leetcode.com/problems/network-delay-time)

In this problem, we are given a network with `N` nodes, labeled from `1` to `N`. Times array `times[i] = (u, v, w)` indicates that there is a directed edge from node `u` to node `v` with time `w`. We need to calculate the time it takes for all nodes to receive a signal sent from `K`. If it is impossible for all the nodes to receive the signal, we return `-1`.

## Approaches

1. [Dijkstra's Algorithm using Priority Queue](#approach-1)
2. [Bellman-Ford Algorithm](#approach-2)

---

## Approach 1: Dijkstra's Algorithm using Priority Queue

Dijkstra's algorithm is ideal for finding the shortest path in a weighted graph with positive weights. We'll use a priority queue (min-heap) to efficiently get the next node with the smallest distance.

**Intuition**:  
- Initialize a priority queue with the source node `K` and a distance of `0`.
- Use an adjacency list to map each node to its edges.
- Process nodes in the order of their distance from the source.
- Update the distance for each adjacent node if a shorter path is found.

```javascript
function networkDelayTime(times, N, K) {
    const adjList = new Map();
    for (const [u, v, w] of times) {
        if (!adjList.has(u)) adjList.set(u, []);
        adjList.get(u).push([v, w]);
    }

    const minHeap = [[0, K]]; // [distance, node]
    const dist = Array(N + 1).fill(Infinity);
    dist[K] = 0;

    while (minHeap.length > 0) {
        const [currentDist, u] = minHeap.shift();
        if (currentDist > dist[u]) continue;

        const neighbors = adjList.get(u) || [];
        for (const [v, w] of neighbors) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                minHeap.push([dist[v], v]);
            }
        }
        minHeap.sort((a, b) => a[0] - b[0]);
    }

    const maxDist = Math.max(...dist.slice(1));
    return maxDist === Infinity ? -1 : maxDist;
}
```

### Time Complexity
- **Time**: \(O((E + V) \log V)\), where \(E\) is the number of edges and \(V\) is the number of vertices due to the priority queue operations.
- **Space**: \(O(V + E)\), which is needed to store the graph and the distances.

## Approach 2: Bellman-Ford Algorithm

Bellman-Ford algorithm can handle negative weight edges and is simpler than Dijkstra's for certain problems. Here, though all weights are positive, it provides an alternative solution.

**Intuition**:
- Initialize the distance to all nodes as infinite, except the source node `K`, which is `0`.
- Relax all edges up to `N-1` times to ensure shortest paths are updated.

```javascript
function networkDelayTime(times, N, K) {
    const dist = Array(N + 1).fill(Infinity);
    dist[K] = 0;

    for (let i = 1; i < N; i++) {
        let changed = false;
        for (const [u, v, w] of times) {
            if (dist[u] != Infinity && dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                changed = true;
            }
        }
        if (!changed) break; // Optimization: Stop if no changes
    }

    const maxDist = Math.max(...dist.slice(1));
    return maxDist === Infinity ? -1 : maxDist;
}
```

### Time Complexity
- **Time**: \(O(V \times E)\), since each edge is relaxed for every vertex.
- **Space**: \(O(V)\), used for storing distances.

Each approach has its own merits. Dijkstra's algorithm is generally more efficient with the use of a priority queue, while Bellman-Ford is versatile for graphs that might have negative edge weights (even though this is not applicable here).

