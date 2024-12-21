# [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Approaches

- [Breadth-First Search (BFS) with Queue](#bfs-with-queue)
- [Dijkstra’s Algorithm with Priority Queue](#dijkstra-algorithm-with-priority-queue)

---

### BFS with Queue

The idea is to explore the graph level by level using a queue, keeping track of the minimum cost to reach each airport. For each airport, we'll explore all possible direct flights to the next connected airports. We will only consider the path if the number of stops is within the allowed `k+1` (since `k` is number of intermediate stops). We'll update the cost of reaching an airport only if the new cost is cheaper than the previously recorded cost.

```typescript
function findCheapestPrice(n: number, flights: number[][], src: number, dst: number, K: number): number {
    // Step 1: Initialize graph
    const graph = new Map<number, number[][]>();
    for (const [u, v, w] of flights) {
        if (!graph.has(u)) graph.set(u, []);
        graph.get(u)!.push([v, w]);
    }

    // Step 2: Initialize the queue
    const queue: [number, number, number][] = [[src, 0, 0]]; // [current node, current cost, number of stops]
    const minCost: number[] = new Array(n).fill(Infinity);
    minCost[src] = 0;

    // Step 3: Perform BFS
    while (queue.length) {
        const [current, cost, stops] = queue.shift()!;
        if (stops > K + 1) continue;       // If stops exceed K+1, continue
        if (graph.has(current)) {
            for (const [next, price] of graph.get(current)!) {
                if (cost + price < minCost[next]) {
                    minCost[next] = cost + price;
                    queue.push([next, cost + price, stops + 1]);
                }
            }
        }
    }

    // Step 4: Return the result
    return minCost[dst] === Infinity ? -1 : minCost[dst];
}
```

**Time Complexity:** O(n * flights) where `n` is the number of nodes and `flights` is the number of edges since we explore each edge in the worst case.

**Space Complexity:** O(n) for the graph and additional storage structures.

---

### Dijkstra’s Algorithm with Priority Queue

A more optimal approach uses Dijkstra’s-like algorithm tailored to track up to `k` stops. We use a priority queue to always expand the cheapest promising paths first. This ensures that we're evaluating the cheapest path to each destination as early as possible, updating our understanding of the path costs efficiently.

```typescript
function findCheapestPrice(n: number, flights: number[][], src: number, dst: number, K: number): number {
    // Step 1: Initialize graph
    const graph = new Map<number, number[][]>();
    for (const [u, v, w] of flights) {
        if (!graph.has(u)) graph.set(u, []);
        graph.get(u)!.push([v, w]);
    }

    // Step 2: Initialize the priority queue (min-heap)
    const heap: [number, number, number][] = [[0, src, 0]]; // [current cost, current node, number of stops]
    const minCost: Map<string, number> = new Map();
    minCost.set(`${src}-0`, 0);

    // Step 3: Perform modified Dijkstra's
    while (heap.length) {
        heap.sort((a, b) => a[0] - b[0]); // Sort by cost (min-heap simulation)
        const [currentCost, currentNode, stops] = heap.shift()!;
        
        // If destination is reached and stops <= K, return the cost
        if (currentNode === dst) return currentCost;
        
        if (stops <= K) {
            if (graph.has(currentNode)) {
                for (const [nextNode, price] of graph.get(currentNode)!) {
                    const newCost = currentCost + price;
                    if (!minCost.has(`${nextNode}-${stops + 1}`) || newCost < minCost.get(`${nextNode}-${stops + 1}`)) {
                        minCost.set(`${nextNode}-${stops + 1}`, newCost);
                        heap.push([newCost, nextNode, stops + 1]);
                    }
                }
            }
        }
    }

    // Step 4: Return result
    return -1;
}
```

**Time Complexity:** O((n + flights) * log n) due to heap operations.

**Space Complexity:** O(n) for the graph, minCost map, and the heap structure.

---

