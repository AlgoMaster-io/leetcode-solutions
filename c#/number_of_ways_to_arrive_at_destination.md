# 1976. [Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

## Approach 1: Dijkstra's Algorithm with Modification

### Solution
csharp
```csharp
// Time Complexity: O(E log V) where E is the number of edges and V is the number of vertices
// Space Complexity: O(V)
using System;
using System.Collections.Generic;

public class Solution {
    public int CountPaths(int n, int[][] roads) {
        const int MOD = 1_000_000_007;
        List<List<int[]>> graph = new List<List<int[]>>();
        for (int i = 0; i < n; i++) {
            graph.Add(new List<int[]>());
        }

        // Construct the graph
        foreach (var road in roads) {
            int u = road[0], v = road[1], time = road[2];
            graph[u].Add(new int[] { v, time });
            graph[v].Add(new int[] { u, time });
        }

        // Dijkstra's algorithm modifications
        long[] minDist = new long[n];
        long[] ways = new long[n];
        Array.Fill(minDist, long.MaxValue);
        ways[0] = 1;
        minDist[0] = 0;

        // Use a priority queue. For .NET, we'll use SortedSet for a similar effect
        SortedSet<(long, int)> pq = new SortedSet<(long, int)>(Comparer<(long, int)>.Create((a, b) =>
            a.Item1 == b.Item1 ? a.Item2.CompareTo(b.Item2) : a.Item1.CompareTo(b.Item1)));
        pq.Add((0, 0)); // {distance, node}

        while (pq.Count > 0) {
            var current = pq.Min;
            pq.Remove(current);
            long currentDist = current.Item1;
            int u = current.Item2;

            if (currentDist > minDist[u]) continue;

            // Traverse neighbors
            foreach (var neighbor in graph[u]) {
                int v = neighbor[0];
                long time = neighbor[1];

                // Relaxation
                if (minDist[u] + time < minDist[v]) {
                    minDist[v] = minDist[u] + time;
                    ways[v] = ways[u];
                    pq.Add((minDist[v], v));
                } else if (minDist[u] + time == minDist[v]) {
                    ways[v] = (ways[v] + ways[u]) % MOD;
                }
            }
        }

        return (int)ways[n - 1];
    }
}
```

This solution efficiently finds the number of shortest paths from node 0 to node n-1 using a modified version of Dijkstra's algorithm that keeps track of the number of ways to arrive at each node. It uses a priority queue to explore the shortest paths and maintains an array to store the number of ways to reach each node at the shortest distance.

