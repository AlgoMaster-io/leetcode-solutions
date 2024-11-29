# 787. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Approach 1: DFS with Pruning

### Solution
csharp
```csharp
// Time Complexity: O(n^k) in the worst case
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    private int result = int.MaxValue;

    public int FindCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        // Create adjacency map for graph edges
        Dictionary<int, Dictionary<int, int>> graph = new Dictionary<int, Dictionary<int, int>>();
        foreach (var flight in flights) {
            if (!graph.ContainsKey(flight[0])) {
                graph[flight[0]] = new Dictionary<int, int>();
            }
            graph[flight[0]][flight[1]] = flight[2];
        }

        // Perform DFS
        Dfs(graph, src, dst, K + 1, 0); // K + 1 because we count steps, not edges

        return result == int.MaxValue ? -1 : result;
    }

    private void Dfs(Dictionary<int, Dictionary<int, int>> graph, int node, int dst, int stops, int cost) {
        if (node == dst) {
            result = cost;
            return;
        }

        if (stops == 0) return; // No more stops available

        if (!graph.ContainsKey(node)) return; // No outgoing flights

        foreach (var entry in graph[node]) {
            int neighbor = entry.Key;
            int price = entry.Value;

            // Pruning: proceed only if this path is cheaper than the best found so far
            if (cost + price > result) continue;

            Dfs(graph, neighbor, dst, stops - 1, cost + price);
        }
    }
}
```

## Approach 2: Bellman-Ford Algorithm

### Solution
csharp
```csharp
// Time Complexity: O(K * E), where E is the number of edges
// Space Complexity: O(n)
using System;

public class Solution {
    public int FindCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        int[] prices = new int[n];
        Array.Fill(prices, int.MaxValue);
        prices[src] = 0;

        // Relax the edges up to K+1 times
        for (int i = 0; i <= K; i++) {
            int[] tempPrices = (int[])prices.Clone();
            foreach (var flight in flights) {
                int u = flight[0], v = flight[1], price = flight[2];
                if (prices[u] == int.MaxValue) continue;

                if (prices[u] + price < tempPrices[v]) {
                    tempPrices[v] = prices[u] + price;
                }
            }
            prices = tempPrices;
        }

        return prices[dst] == int.MaxValue ? -1 : prices[dst];
    }
}
```

## Approach 3: Dijkstra's Algorithm with Priority Queue

### Solution
csharp
```csharp
// Time Complexity: O(E + VlogV), where E is the number of edges and V is the number of vertices
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int FindCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        // Create an adjacency list for the graph
        Dictionary<int, List<int[]>> graph = new Dictionary<int, List<int[]>>();
        foreach (var flight in flights) {
            if (!graph.ContainsKey(flight[0])) {
                graph[flight[0]] = new List<int[]>();
            }
            graph[flight[0]].Add(new int[] { flight[1], flight[2] });
        }

        // Priority queue will hold entries of (cost, node, stops remaining)
        SortedSet<int[]> pq = new SortedSet<int[]>(Comparer<int[]>.Create((a, b) => a[0] == b[0] ? a[1].CompareTo(b[1]) : a[0].CompareTo(b[0])));
        pq.Add(new int[] { 0, src, K + 1 });

        while (pq.Count > 0) {
            var current = pq.Min;
            pq.Remove(current);
            int cost = current[0];
            int node = current[1];
            int stopsRemaining = current[2];

            if (node == dst) {
                return cost;
            }

            if (stopsRemaining > 0) {
                if (!graph.ContainsKey(node)) continue;
                foreach (var neighbor in graph[node]) {
                    int nextNode = neighbor[0];
                    int priceToNext = neighbor[1];
                    pq.Add(new int[] { cost + priceToNext, nextNode, stopsRemaining - 1 });
                }
            }
        }

        return -1; // No valid path found
    }
}
```

