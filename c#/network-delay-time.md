# [Leetcode 743: Network Delay Time](https://leetcode.com/problems/network-delay-time/)

## Approaches:
- [Approach 1: Dijkstra's Algorithm using Min-Heap (Priority Queue)](#approach-1-dijkstras-algorithm-using-min-heap-priority-queue)
- [Approach 2: Bellman-Ford Algorithm](#approach-2-bellman-ford-algorithm)

## Approach 1: Dijkstra's Algorithm using Min-Heap (Priority Queue)

### Intuition
Dijkstra's algorithm is well-suited for finding the shortest path from a single source to all other vertices in a graph with non-negative weights, which fits the problem specification. We will simulate the network delay as the shortest path from the starting node to all other nodes. We need to use a priority queue to always extend the shortest known distances first.

### Code
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public int NetworkDelayTime(int[][] times, int n, int k) {
        // Build the adjacency list to represent the graph
        Dictionary<int, List<(int, int)>> graph = new Dictionary<int, List<(int, int)>>();
        foreach (var time in times) {
            if (!graph.ContainsKey(time[0])) {
                graph[time[0]] = new List<(int, int)>();
            }
            graph[time[0]].Add((time[1], time[2]));
        }

        // Use a priority queue (min heap) to hold the next shortest edge
        SortedSet<(int, int)> minHeap = new SortedSet<(int, int)>();

        // Dictionary to hold the shortest distance to each node
        Dictionary<int, int> dist = new Dictionary<int, int>();

        // Start from node k with distance 0
        minHeap.Add((0, k));

        while (minHeap.Count > 0) {
            var (currentDist, currentNode) = minHeap.Min;
            minHeap.Remove(minHeap.Min);

            if (!dist.ContainsKey(currentNode)) {
                dist[currentNode] = currentDist;

                // Look at all neighbours of the currentNode
                if (graph.ContainsKey(currentNode)) {
                    foreach (var (neighbor, time) in graph[currentNode]) {
                        if (!dist.ContainsKey(neighbor)) {
                            minHeap.Add((currentDist + time, neighbor));
                        }
                    }
                }
            }
        }

        // If we have reached all nodes, compute the delay
        if (dist.Count == n) {
            return dist.Values.Max();
        }

        // If not all nodes are reachable, return -1
        return -1;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(E + N log N), where E is the number of edges and N is the number of nodes. Constructing the graph takes O(E), and each operation in the priority queue is log N.
- **Space Complexity**: O(N + E), for storing the graph and the priority queue.

## Approach 2: Bellman-Ford Algorithm

### Intuition
Bellman-Ford is another algorithm to calculate the shortest paths from a single source in a graph and can handle graphs with negative weights. Although we don't need it in this problem, it's a good alternative when edges can be negative. Its strength lies in its simplicity and its ability to work with more general scenarios than Dijkstra.

### Code
```csharp
using System;

public class Solution {
    public int NetworkDelayTime(int[][] times, int n, int k) {
        // Initialize distances with a large number (infinity)
        int[] dist = new int[n + 1];
        Array.Fill(dist, int.MaxValue);
        dist[k] = 0;

        // Relax each edge up to (n - 1) times
        for (int i = 0; i < n - 1; ++i) {
            foreach (var time in times) {
                int u = time[0], v = time[1], w = time[2];
                if (dist[u] != int.MaxValue && dist[u] + w < dist[v]) {
                    dist[v] = dist[u] + w;
                }
            }
        }

        // Find the maximum distance from the source node to calculate the delay
        int maxDist = 0;
        for (int i = 1; i <= n; ++i) {
            if (dist[i] == int.MaxValue) return -1;  // Not all nodes are reachable
            maxDist = Math.Max(maxDist, dist[i]);
        }

        return maxDist;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(N * E), where E is the number of edges and N is the number of nodes. Each edge is relaxed (N - 1) times.
- **Space Complexity**: O(N), for storing the distance of each node.

By understanding both Dijkstra and Bellman-Ford, you can choose the appropriate algorithm based on the constraints and properties of the input graph.

