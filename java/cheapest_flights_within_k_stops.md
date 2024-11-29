# 787. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Approach 1: DFS with Pruning

### Solution
```java
// Time Complexity: O(n^k) in the worst case
// Space Complexity: O(n)
import java.util.HashMap;
import java.util.Map;

public class Solution {
    int result = Integer.MAX_VALUE;

    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        // Create adjacency map for graph edges
        Map<Integer, Map<Integer, Integer>> graph = new HashMap<>();
        for (int[] flight : flights) {
            graph.computeIfAbsent(flight[0], k -> new HashMap<>()).put(flight[1], flight[2]);
        }

        // Perform DFS
        dfs(graph, src, dst, K + 1, 0); // K + 1 because we count steps, not edges

        return result == Integer.MAX_VALUE ? -1 : result;
    }

    private void dfs(Map<Integer, Map<Integer, Integer>> graph, int node, int dst, int stops, int cost) {
        if (node == dst) {
            result = cost;
            return;
        }

        if (stops == 0) return; // No more stops available

        if (!graph.containsKey(node)) return; // No outgoing flights

        for (Map.Entry<Integer, Integer> entry : graph.get(node).entrySet()) {
            int neighbor = entry.getKey();
            int price = entry.getValue();

            // Pruning: proceed only if this path is cheaper than the best found so far
            if (cost + price > result) continue;

            dfs(graph, neighbor, dst, stops - 1, cost + price);
        }
    }
}
```

## Approach 2: Bellman-Ford Algorithm

### Solution
```java
// Time Complexity: O(K * E), where E is the number of edges
// Space Complexity: O(n)
import java.util.Arrays;

public class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        int[] prices = new int[n];
        Arrays.fill(prices, Integer.MAX_VALUE);
        prices[src] = 0;

        // Relax the edges up to K+1 times
        for (int i = 0; i <= K; i++) {
            int[] tempPrices = Arrays.copyOf(prices, n);
            for (int[] flight : flights) {
                int u = flight[0], v = flight[1], price = flight[2];
                if (prices[u] == Integer.MAX_VALUE) continue;

                if (prices[u] + price < tempPrices[v]) {
                    tempPrices[v] = prices[u] + price;
                }
            }
            prices = tempPrices;
        }

        return prices[dst] == Integer.MAX_VALUE ? -1 : prices[dst];
    }
}
```

## Approach 3: Dijkstra's Algorithm with Priority Queue

### Solution
```java
// Time Complexity: O(E + VlogV), where E is the number of edges and V is the number of vertices
// Space Complexity: O(n)
import java.util.*;

public class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        // Create an adjacency list for the graph
        Map<Integer, List<int[]>> graph = new HashMap<>();
        for (int[] flight : flights) {
            graph.computeIfAbsent(flight[0], k -> new ArrayList<>()).add(new int[]{flight[1], flight[2]});
        }

        // Priority queue will hold entries of (cost, node, stops remaining)
        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        pq.offer(new int[]{0, src, K + 1});

        while (!pq.isEmpty()) {
            int[] current = pq.poll();
            int cost = current[0], node = current[1], stopsRemaining = current[2];

            if (node == dst) {
                return cost;
            }

            if (stopsRemaining > 0) {
                if (!graph.containsKey(node)) continue;
                for (int[] neighbor : graph.get(node)) {
                    int nextNode = neighbor[0];
                    int priceToNext = neighbor[1];
                    pq.offer(new int[]{cost + priceToNext, nextNode, stopsRemaining - 1});
                }
            }
        }

        return -1; // No valid path found
    }
}
```

