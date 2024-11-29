# 743. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
csharp
```csharp
// Time Complexity: O((V + E) log V)
// Space Complexity: O(V + E)
using System;
using System.Collections.Generic;

public class Solution {
    public int NetworkDelayTime(int[][] times, int n, int k) {
        // Create a graph as adjacency list representation
        var graph = new Dictionary<int, List<(int, int)>>();
        foreach (var time in times) {
            if (!graph.ContainsKey(time[0])) {
                graph[time[0]] = new List<(int, int)>();
            }
            graph[time[0]].Add((time[1], time[2]));
        }

        // Priority Queue to hold nodes as pairs of (time, node)
        var pq = new SortedSet<(int, int)>();
        pq.Add((0, k));

        // Dictionary to track the minimum time to reach each node
        var minTimeToReach = new Dictionary<int, int>();

        // Dijkstra's algorithm
        while (pq.Count > 0) {
            var current = pq.Min;
            pq.Remove(current);

            int currentTime = current.Item1;
            int currentNode = current.Item2;

            if (minTimeToReach.ContainsKey(currentNode)) 
                continue;

            minTimeToReach[currentNode] = currentTime;
            if (graph.ContainsKey(currentNode)) {
                foreach (var adj in graph[currentNode]) {
                    int nextNode = adj.Item1;
                    int travelTime = adj.Item2;
                    if (!minTimeToReach.ContainsKey(nextNode)) {
                        pq.Add((currentTime + travelTime, nextNode));
                    }
                }
            }
        }

        // Determine the maximum time needed
        if (minTimeToReach.Count != n) 
            return -1;

        int maxTime = 0;
        foreach (var time in minTimeToReach.Values) {
            maxTime = Math.Max(maxTime, time);
        }

        return maxTime;
    }
}
```

## Approach 2: Bellman-Ford Algorithm

### Solution
csharp
```csharp
// Time Complexity: O(V * E)
// Space Complexity: O(V)
using System;

public class Solution {
    public int NetworkDelayTime(int[][] times, int n, int k) {
        int[] dist = new int[n + 1];
        Array.Fill(dist, int.MaxValue);
        dist[k] = 0;

        // Relaxation process for V-1 times
        for (int i = 1; i < n; i++) {
            foreach (var edge in times) {
                int u = edge[0], v = edge[1], w = edge[2];
                if (dist[u] != int.MaxValue && dist[u] + w < dist[v]) {
                    dist[v] = dist[u] + w;
                }
            }
        }

        int maxTime = 0;
        for (int i = 1; i <= n; i++) {
            if (dist[i] == int.MaxValue) 
                return -1;
            maxTime = Math.Max(maxTime, dist[i]);
        }

        return maxTime;
    }
}
```

## Approach 3: Dynamic Programming with Floyd-Warshall Algorithm

### Solution
csharp
```csharp
// Time Complexity: O(V^3)
// Space Complexity: O(V^2)
using System;

public class Solution {
    public int NetworkDelayTime(int[][] times, int n, int k) {
        int[,] dp = new int[n+1, n+1];
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= n; j++) {
                dp[i, j] = (i == j) ? 0 : int.MaxValue / 2;
            }
        }

        // Initialize graph with given times
        foreach (var time in times) {
            dp[time[0], time[1]] = time[2];
        }

        // Floyd-Warshall algorithm for shortest paths
        for (int t = 1; t <= n; t++) {
            for (int u = 1; u <= n; u++) {
                for (int v = 1; v <= n; v++) {
                    if (dp[u, t] < int.MaxValue && dp[t, v] < int.MaxValue) {
                        dp[u, v] = Math.Min(dp[u, v], dp[u, t] + dp[t, v]);
                    }
                }
            }
        }

        // Calculate the longest shortest path from the start node k
        int maxTime = 0;
        for (int i = 1; i <= n; i++) {
            if (dp[k, i] >= int.MaxValue) 
                return -1;
            maxTime = Math.Max(maxTime, dp[k, i]);
        }

        return maxTime;
    }
}
```

