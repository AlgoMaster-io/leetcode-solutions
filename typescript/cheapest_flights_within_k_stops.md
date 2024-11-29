# 787. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Approach 1: DFS with Pruning

### Solution
typescript
```typescript
// Time Complexity: O(n^k) in the worst case
// Space Complexity: O(n)

type Graph = Map<number, Map<number, number>>;

class Solution {
    private result: number = Number.MAX_SAFE_INTEGER;

    public findCheapestPrice(n: number, flights: number[][], src: number, dst: number, K: number): number {
        const graph: Graph = new Map();

        // Create adjacency map for graph edges
        for (const flight of flights) {
            if (!graph.has(flight[0])) {
                graph.set(flight[0], new Map());
            }
            graph.get(flight[0])!.set(flight[1], flight[2]);
        }

        // Perform DFS
        this.dfs(graph, src, dst, K + 1, 0); // K + 1 because we count steps, not edges

        return this.result === Number.MAX_SAFE_INTEGER ? -1 : this.result;
    }

    private dfs(graph: Graph, node: number, dst: number, stops: number, cost: number): void {
        if (node === dst) {
            this.result = cost;
            return;
        }

        if (stops === 0) return; // No more stops available

        if (!graph.has(node)) return; // No outgoing flights

        for (const [neighbor, price] of graph.get(node)!.entries()) {
            // Pruning: proceed only if this path is cheaper than the best found so far
            if (cost + price > this.result) continue;

            this.dfs(graph, neighbor, dst, stops - 1, cost + price);
        }
    }
}
```

## Approach 2: Bellman-Ford Algorithm

### Solution
typescript
```typescript
// Time Complexity: O(K * E), where E is the number of edges
// Space Complexity: O(n)

class Solution {
    public findCheapestPrice(n: number, flights: number[][], src: number, dst: number, K: number): number {
        const prices: number[] = new Array(n).fill(Number.MAX_SAFE_INTEGER);
        prices[src] = 0;

        // Relax the edges up to K+1 times
        for (let i = 0; i <= K; i++) {
            const tempPrices: number[] = [...prices];
            for (const flight of flights) {
                const [u, v, price] = flight;
                if (prices[u] === Number.MAX_SAFE_INTEGER) continue;

                if (prices[u] + price < tempPrices[v]) {
                    tempPrices[v] = prices[u] + price;
                }
            }
            prices = tempPrices;
        }

        return prices[dst] === Number.MAX_SAFE_INTEGER ? -1 : prices[dst];
    }
}
```

## Approach 3: Dijkstra's Algorithm with Priority Queue

### Solution
typescript
```typescript
// Time Complexity: O(E + VlogV), where E is the number of edges and V is the number of vertices
// Space Complexity: O(n)

type Graph = Map<number, Array<[number, number]>>;

class Solution {
    public findCheapestPrice(n: number, flights: number[][], src: number, dst: number, K: number): number {
        const graph: Graph = new Map();

        // Create an adjacency list for the graph
        for (const flight of flights) {
            const [u, v, w] = flight;
            if (!graph.has(u)) {
                graph.set(u, []);
            }
            graph.get(u)!.push([v, w]);
        }

        // Priority queue will hold entries of (cost, node, stops remaining)
        const pq: PriorityQueue<number[]> = new PriorityQueue((a, b) => a[0] - b[0]);
        
        pq.offer([0, src, K + 1]);
        
        while (!pq.isEmpty()) {
            const [cost, node, stopsRemaining] = pq.poll();
            if (node === dst) {
                return cost;
            }

            if (stopsRemaining > 0) {
                if (!graph.has(node)) continue;
                for (const [nextNode, priceToNext] of graph.get(node)!) {
                    pq.offer([cost + priceToNext, nextNode, stopsRemaining - 1]);
                }
            }
        }

        return -1; // No valid path found
    }
}

// Priority Queue implementation for TypeScript
class PriorityQueue<T> {
    private data: T[];
    private comparator: (a: T, b: T) => number;

    constructor(comparator: (a: T, b: T) => number) {
        this.data = [];
        this.comparator = comparator;
    }

    offer(element: T): void {
        this.data.push(element);
        this.data.sort(this.comparator);
    }
    
    poll(): T | undefined {
        return this.data.shift();
    }
    
    isEmpty(): boolean {
        return this.data.length === 0;
    }
}
```

