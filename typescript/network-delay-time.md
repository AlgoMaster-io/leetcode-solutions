# [Network Delay Time - LeetCode](https://leetcode.com/problems/network-delay-time/)

## Table of Contents
- [Approach 1: Naive Approach (Floyd-Warshall Algorithm)](#approach-1-naive-approach-floyd-warshall-algorithm)
- [Approach 2: Using Dijkstra's Algorithm (Priority Queue)](#approach-2-using-dijkstras-algorithm-priority-queue)
- [Approach 3: Improved Dijkstra’s Algorithm with Min-Heap](#approach-3-improved-dijkstras-algorithm-with-min-heap)

### Approach 1: Naive Approach (Floyd-Warshall Algorithm)

#### Intuition
The Floyd-Warshall algorithm is a classic algorithm used to find shortest paths between all pairs of nodes in a graph. While this approach checks every possible path, it is not efficient for large graphs but could serve as a foundational understanding of graph traversal and weights.

1. Initialize a 2D distances array where distance from each node to itself is 0, and other pairs are initially set to infinity.
2. Update the immediate edge distances with the given weights.
3. Iterate through each pair of nodes `(i, j)` to find a shorter path via another node `k` if possible.
4. Finally, calculate the maximum delay to reach any node from the given starting node `k`.

```typescript
function networkDelayTime(times: number[][], n: number, k: number): number {
    const INF = Infinity;
    // Initialize distances array
    const dist = Array.from({ length: n + 1 }, () => Array(n + 1).fill(INF));
    for (let i = 1; i <= n; i++) dist[i][i] = 0;

    // Fill in the direct connections
    for (const [u, v, w] of times) {
        dist[u][v] = w;
    }

    // Floyd-Warshall
    for (let via = 1; via <= n; via++) {
        for (let i = 1; i <= n; i++) {
            for (let j = 1; j <= n; j++) {
                if (dist[i][via] !== INF && dist[via][j] !== INF) {
                    dist[i][j] = Math.min(dist[i][j], dist[i][via] + dist[via][j]);
                }
            }
        }
    }

    const maxDist = Math.max(...dist[k].slice(1));
    return maxDist === INF ? -1 : maxDist;
}
```

**Time Complexity:** O(n^3)
- Due to three nested loops in the Floyd-Warshall algorithm.

**Space Complexity:** O(n^2)
- For storing the distance matrix.

### Approach 2: Using Dijkstra's Algorithm (Priority Queue)

#### Intuition
Utilize Dijkstra's algorithm which efficiently finds the shortest path from the source to all nodes, but instead of applying it to all nodes, we focus only on the starting node `k`.

1. Use an adjacency list to represent the graph.
2. Utilize a priority queue (min-heap) to store nodes with their current tentative distances.
3. Update distances using a "relaxation" process.
4. Extract the maximum distance among all reached nodes at the end.

```typescript
function networkDelayTime(times: number[][], n: number, k: number): number {
    const adjList: Map<number, [number, number][]> = new Map();
    // Build adjacency list
    for (const [u, v, w] of times) {
        if (!adjList.has(u)) adjList.set(u, []);
        adjList.get(u)!.push([v, w]);
    }

    // Min-heap with [cost, node]
    const minHeap: [number, number][] = [[0, k]];
    const dist: number[] = Array(n + 1).fill(Infinity);
    dist[k] = 0;

    while (minHeap.length > 0) {
        const [currDistance, u] = minHeap.shift()!;
        // If this path is longer than the shortest known, ignore
        if (currDistance > dist[u]) continue;

        const neighbors = adjList.get(u) ?? [];
        for (const [v, weight] of neighbors) {
            const newDistance = currDistance + weight;
            if (newDistance < dist[v]) {
                dist[v] = newDistance;
                minHeap.push([newDistance, v]);
                // Keep the minHeap sorted by cost
                minHeap.sort(([c1, _n1], [c2, _n2]) => c1 - c2);
            }
        }
    }

    const maxDist = Math.max(...dist.slice(1));
    return maxDist === Infinity ? -1 : maxDist;
}
```

**Time Complexity:** O((V+E) log V)
- Due to min-heap operations with vertices \(V\) and edges \(E\).

**Space Complexity:** O(V+E)
- For storing the adjacency list and distances.

### Approach 3: Improved Dijkstra's Algorithm with Min-Heap

#### Intuition
Further optimize the solution by using a proper priority queue implementation to maintain and extract minimum elements efficiently.

- Use a min-heap to efficiently retrieve the next node with the smallest tentative distance, reducing the time complexity related to sorting operations previously necessary.

**Note:** For this implementation, a separate min-heap utility class should be integrated or assumed from a library to handle priority queue operations efficiently, which would be outside the typical contents of the main solution code.

```typescript
class MinHeap {
    private heap: [number, number][];
    
    constructor() {
        this.heap = [];
    }

    public insert(node: [number, number]) {
        this.heap.push(node);
        this.bubbleUp(this.heap.length - 1);
    }

    public extractMin(): [number, number] | undefined {
        if (this.heap.length === 0) return undefined;
        this.swap(0, this.heap.length - 1);
        const min = this.heap.pop();
        this.bubbleDown(0);
        return min;
    }

    public isEmpty(): boolean {
        return this.heap.length === 0;
    }

    private bubbleUp(index: number) {
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            if (this.heap[parentIndex][0] <= this.heap[index][0]) break;
            this.swap(parentIndex, index);
            index = parentIndex;
        }
    }

    private bubbleDown(index: number) {
        while (2 * index + 1 < this.heap.length) {
            let smallest = 2 * index + 1;
            if (2 * index + 2 < this.heap.length && this.heap[2 * index + 2][0] < this.heap[smallest][0]) {
                smallest = 2 * index + 2;
            }
            if (this.heap[index][0] <= this.heap[smallest][0]) break;
            this.swap(index, smallest);
            index = smallest;
        }
    }

    private swap(i: number, j: number) {
        [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
    }
}

function networkDelayTime(times: number[][], n: number, k: number): number {
    const adjList: Map<number, [number, number][]> = new Map();
    for (const [u, v, w] of times) {
        if (!adjList.has(u)) adjList.set(u, []);
        adjList.get(u)!.push([v, w]);
    }

    const minHeap = new MinHeap();
    minHeap.insert([0, k]);
    const dist: number[] = Array(n + 1).fill(Infinity);
    dist[k] = 0;

    while (!minHeap.isEmpty()) {
        const [currDistance, u] = minHeap.extractMin()!;
        if (currDistance > dist[u]) continue;

        const neighbors = adjList.get(u) ?? [];
        for (const [v, weight] of neighbors) {
            const newDistance = currDistance + weight;
            if (newDistance < dist[v]) {
                dist[v] = newDistance;
                minHeap.insert([newDistance, v]);
            }
        }
    }

    const maxDist = Math.max(...dist.slice(1));
    return maxDist === Infinity ? -1 : maxDist;
}
```

**Time Complexity:** O((V + E) log V)
- Dijkstra’s algorithm using a min-heap for efficient priority queue operations.

**Space Complexity:** O(V + E)
- For adjacency list and distance tracking.

