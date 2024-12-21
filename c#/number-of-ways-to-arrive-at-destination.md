# [1976. Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

## Approaches

- [Approach 1: Dijkstra's Algorithm with Path Counting](#approach-1-dijkstras-algorithm-with-path-counting)

## Approach 1: Dijkstra's Algorithm with Path Counting

### Intuition
The problem revolves around finding the number of distinct paths with the minimum travel time from a source node to a destination node in a graph. This can be effectively solved using Dijkstra's algorithm, which finds the shortest paths from a source node to all other nodes. However, to count the number of such shortest paths, we need to slightly adjust the usual Dijkstra's approach:

1. Utilize a min-heap to always extend the shortest available path.
2. Maintain an array `ways[]` where `ways[node]` would store the number of ways you can reach `node` with the smallest travel time found so far.
3. If during the relaxation step, a new shortest path to a node is discovered, update the shortest time and reset `ways[node]`.
4. If another optimal path is found (time equals the best-known time), simply add to `ways[node]`.

### Algorithm
1. Use Dijkstra's algorithm to find the shortest path.
2. Track the number of ways to reach each node with the shortest path time using an integer array `ways[]`.
3. For each node, update the shortest time and number of ways appropriately.

### Detailed Code

```csharp
using System;
using System.Collections.Generic;

public class Solution {
    
    private const int MOD = 1000000007;
    
    public int CountPaths(int n, int[][] roads) {
        List<(int, int)>[] graph = new List<(int, int)>[n];
        
        for (int i = 0; i < n; i++) {
            graph[i] = new List<(int, int)>();
        }
        
        foreach (var road in roads) {
            int u = road[0], v = road[1], time = road[2];
            graph[u].Add((v, time));
            graph[v].Add((u, time));
        }
        
        int[] dist = new int[n]; // shortest time to reach each node
        int[] ways = new int[n]; // number of ways to reach each node with the shortest time
        Array.Fill(dist, int.MaxValue);
        Array.Fill(ways, 0);
        
        // PriorityQueue of (time, node)
        SortedSet<(int, int)> pq = new SortedSet<(int, int)>();
        pq.Add((0, 0)); // (time, node)
        dist[0] = 0;
        ways[0] = 1;

        while (pq.Count > 0) {
            var current = pq.Min;
            pq.Remove(current);
            int time = current.Item1;
            int u = current.Item2;

            if (time > dist[u]) continue;
            
            foreach (var (v, t) in graph[u]) {
                int newDist = time + t;

                if (newDist < dist[v]) {
                    dist[v] = newDist;
                    pq.Add((newDist, v));
                    ways[v] = ways[u];

                } else if (newDist == dist[v]) {
                    ways[v] = (ways[v] + ways[u]) % MOD;
                }
            }
        }
        
        return ways[n - 1];
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(E log V), where V is the number of nodes and E is the number of edges, because we're using a priority queue to manage the nodes to visit.
- **Space Complexity**: O(V + E), for storing the graph structure, distance and ways arrays, and priority queue.

This approach efficiently calculates both the minimum path lengths and the count of such paths using the benefits of Dijkstra's algorithm with a slight modification to track the number of optimal paths.

