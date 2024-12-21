# [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Approaches
1. [Modified Depth-First Search with Pruning](#approach-1-modified-dfs)
2. [Bellman-Ford Algorithm (Dynamic Programming)](#approach-2-bellman-ford)
3. [Dijkstra's Algorithm with Priority Queue](#approach-3-dijkstra)

### Approach 1: Modified Depth-First Search with Pruning

Intuition:
This approach involves using a modified DFS algorithm to explore all possible routes from the source to the destination, while pruning unnecessary paths based on constraints (i.e., the number of stops).

1. Use a recursive DFS method to visit all nodes.
2. If the number of stops exceeds `K`, return as we cannot make more stops.
3. Track the currently accumulated cost; if it exceeds the known minimum cost for reaching the destination, prune that path.
4. Use the adjacency list to explore every neighboring node recursively.
5. Update the result whenever a valid path with a cost lower than the current known minimum is found.

```java
public class Solution {
    private int minCost = Integer.MAX_VALUE;
    
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        Map<Integer, List<int[]>> graph = new HashMap<>();
        for (int[] flight : flights) {
            graph.putIfAbsent(flight[0], new ArrayList<>());
            graph.get(flight[0]).add(new int[]{flight[1], flight[2]});
        }
        
        dfs(graph, src, dst, K + 1, 0);
        return minCost == Integer.MAX_VALUE ? -1 : minCost;
    }
    
    private void dfs(Map<Integer, List<int[]>> graph, int node, int dst, int stops, int cost) {
        if (node == dst) {  // If destination is reached
            minCost = cost;
            return;
        }
        if (stops == 0) return; // No more stops allowed
        
        if (!graph.containsKey(node)) return;
        for (int[] neighbor : graph.get(node)) {
            // Pruning
            if (cost + neighbor[1] > minCost) continue;
            dfs(graph, neighbor[0], dst, stops - 1, cost + neighbor[1]);
        }
    }
}
```

**Time Complexity:** \(O(K \times E)\) where \(E\) is the number of flights, as for every node we are exploring up to K+1 times.

**Space Complexity:** \(O(V + E)\) where \(V\) is the number of vertices. Storage for the graph and call stack for recursion.

---

### Approach 2: Bellman-Ford Algorithm (Dynamic Programming)

Intuition:
Bellman-Ford is a classic dynamic programming algorithm that can be adapted to this problem with modifications to accommodate the stop constraint.

1. Use a 2D DP array `dp` where `dp[i][j]` represents the minimum cost to reach city `j` using at most `i` stops.
2. Initialize `dp[0][src]` to 0 because starting at the source requires no cost, and the rest to infinity.
3. Update costs for flights up to `K+1` times to ensure the number of stops doesn't exceed `K`.

```java
public class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        int[][] dp = new int[K + 2][n];
        for (int i = 0; i <= K + 1; i++) {
            Arrays.fill(dp[i], Integer.MAX_VALUE);
        }
        dp[0][src] = 0;
        
        for (int i = 1; i <= K + 1; i++) {
            for (int j = 0; j < n; j++) {
                dp[i][j] = dp[i - 1][j]; // carry forward previous values
            }
            for (int[] flight : flights) {
                int u = flight[0], v = flight[1], w = flight[2];
                if (dp[i - 1][u] != Integer.MAX_VALUE) {
                    dp[i][v] = Math.min(dp[i][v], dp[i - 1][u] + w);
                }
            }
        }
        return dp[K + 1][dst] == Integer.MAX_VALUE ? -1 : dp[K + 1][dst];
    }
}
```

**Time Complexity:** \(O(K \times E)\) where \(E\) is the number of flights, iterating through flights for `K+1` times.

**Space Complexity:** \(O(K \times V)\) for storing DP table.

---

### Approach 3: Dijkstra's Algorithm with Priority Queue

Intuition:
A priority queue (min-heap) can help us always extend the least costly current flight path using a variant of Dijkstra's algorithm that's modified to account for the maximum number of allowed stops.

1. Use a priority queue to always select the current minimum cost path.
2. Keep track of costs and number of stops made.
3. If we reach a destination before exceeding `K` stops, we found the solution.
4. Use an array to store the minimum cost to reach each city within a certain number of stops.

```java
import java.util.*;

public class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        // PriorityQueue element format {cost, city, stops}
        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        Map<Integer, List<int[]>> graph = new HashMap<>();
        
        for (int[] flight : flights) {
            graph.putIfAbsent(flight[0], new ArrayList<>());
            graph.get(flight[0]).add(new int[]{flight[1], flight[2]});
        }
        
        int[] minCost = new int[n];
        Arrays.fill(minCost, Integer.MAX_VALUE);
        pq.offer(new int[]{0, src, 0}); // {cost, city, stops}
        
        while (!pq.isEmpty()) {
            int[] current = pq.poll();
            int cost = current[0];
            int city = current[1];
            int stops = current[2];
            
            // If destination reached with allowed stops
            if (city == dst) return cost;
            
            if (stops <= K) {
                for (int[] next : graph.getOrDefault(city, new ArrayList<>())) {
                    int newCost = cost + next[1];
                    if (newCost < minCost[next[0]]) {
                        minCost[next[0]] = newCost;
                        pq.offer(new int[]{newCost, next[0], stops + 1});
                    }
                }
            }
        }
        return -1;
    }
}
```

**Time Complexity:** \(O(E \log V)\) where \(E\) is the number of flights. The log factor comes from the priority queue.

**Space Complexity:** \(O(V \times E)\) due to adjacency list and priority queue.

