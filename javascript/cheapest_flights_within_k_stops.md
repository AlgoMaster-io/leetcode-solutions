# 787. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Approach 1: DFS with Pruning

### Solution
```javascript
// Time Complexity: O(n^k) in the worst case
// Space Complexity: O(n)

class Solution {
  constructor() {
    this.result = Number.MAX_VALUE;
  }

  findCheapestPrice(n, flights, src, dst, K) {
    // Create adjacency map for graph edges
    const graph = new Map();
    for (const [u, v, price] of flights) {
      if (!graph.has(u)) graph.set(u, new Map());
      graph.get(u).set(v, price);
    }

    // Perform DFS
    this.dfs(graph, src, dst, K + 1, 0); // K + 1 because we count steps, not edges

    return this.result === Number.MAX_VALUE ? -1 : this.result;
  }

  dfs(graph, node, dst, stops, cost) {
    if (node === dst) {
      this.result = cost;
      return;
    }

    if (stops === 0) return; // No more stops available

    if (!graph.has(node)) return; // No outgoing flights

    for (const [neighbor, price] of graph.get(node)) {
      // Pruning: proceed only if this path is cheaper than the best found so far
      if (cost + price > this.result) continue;

      this.dfs(graph, neighbor, dst, stops - 1, cost + price);
    }
  }
}
```

## Approach 2: Bellman-Ford Algorithm

### Solution
```javascript
// Time Complexity: O(K * E), where E is the number of edges
// Space Complexity: O(n)

class Solution {
  findCheapestPrice(n, flights, src, dst, K) {
    const prices = new Array(n).fill(Number.MAX_VALUE);
    prices[src] = 0;

    // Relax the edges up to K+1 times
    for (let i = 0; i <= K; i++) {
      const tempPrices = [...prices];
      for (const [u, v, price] of flights) {
        if (prices[u] === Number.MAX_VALUE) continue;

        if (prices[u] + price < tempPrices[v]) {
          tempPrices[v] = prices[u] + price;
        }
      }
      prices = tempPrices;
    }

    return prices[dst] === Number.MAX_VALUE ? -1 : prices[dst];
  }
}
```

## Approach 3: Dijkstra's Algorithm with Priority Queue

### Solution
```javascript
// Time Complexity: O(E + VlogV), where E is the number of edges and V is the number of vertices
// Space Complexity: O(n)

class Solution {
  findCheapestPrice(n, flights, src, dst, K) {
    // Create an adjacency list for the graph
    const graph = new Map();
    for (const [u, v, price] of flights) {
      if (!graph.has(u)) graph.set(u, []);
      graph.get(u).push([v, price]);
    }

    // Priority queue will hold entries of (cost, node, stops remaining)
    const pq = new MinPriorityQueue({ priority: (entry) => entry[0] });
    pq.enqueue([0, src, K + 1]);

    while (!pq.isEmpty()) {
      const current = pq.dequeue().element;
      const [cost, node, stopsRemaining] = current;

      if (node === dst) {
        return cost;
      }

      if (stopsRemaining > 0) {
        if (!graph.has(node)) continue;
        for (const [nextNode, priceToNext] of graph.get(node)) {
          pq.enqueue([cost + priceToNext, nextNode, stopsRemaining - 1]);
        }
      }
    }

    return -1; // No valid path found
  }
}
```


