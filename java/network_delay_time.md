# 743. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
```java
// Time Complexity: O((V + E) log V)
// Space Complexity: O(V + E)
import java.util.*;

public class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        // Create a graph as adjacency list representation
        Map<Integer, List<int[]>> graph = new HashMap<>();
        for (int[] time : times) {
            graph.putIfAbsent(time[0], new ArrayList<>());
            graph.get(time[0]).add(new int[]{time[1], time[2]});
        }

        // Priority Queue to hold nodes as pairs of (time, node)
        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        pq.add(new int[]{0, k});

        // Map to track the minimum time to reach each node
        Map<Integer, Integer> minTimeToReach = new HashMap<>();

        // Dijkstra's algorithm
        while (!pq.isEmpty()) {
            int[] current = pq.poll();
            int currentTime = current[0];
            int currentNode = current[1];

            if (minTimeToReach.containsKey(currentNode)) 
                continue;

            minTimeToReach.put(currentNode, currentTime);
            if (graph.containsKey(currentNode)) {
                for (int[] adj : graph.get(currentNode)) {
                    int nextNode = adj[0];
                    int travelTime = adj[1];
                    if (!minTimeToReach.containsKey(nextNode)) {
                        pq.add(new int[]{currentTime + travelTime, nextNode});
                    }
                }
            }
        }

        // Determine the maximum time needed
        if (minTimeToReach.size() != n) 
            return -1;

        int maxTime = 0;
        for (int time : minTimeToReach.values()) {
            maxTime = Math.max(maxTime, time);
        }

        return maxTime;
    }
}
```

## Approach 2: Bellman-Ford Algorithm

### Solution
```java
// Time Complexity: O(V * E)
// Space Complexity: O(V)
import java.util.Arrays;

public class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        int[] dist = new int[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[k] = 0;

        // Relaxation process for V-1 times
        for (int i = 1; i < n; i++) {
            for (int[] edge : times) {
                int u = edge[0], v = edge[1], w = edge[2];
                if (dist[u] != Integer.MAX_VALUE && dist[u] + w < dist[v]) {
                    dist[v] = dist[u] + w;
                }
            }
        }

        int maxTime = 0;
        for (int i = 1; i <= n; i++) {
            if (dist[i] == Integer.MAX_VALUE) 
                return -1;
            maxTime = Math.max(maxTime, dist[i]);
        }

        return maxTime;
    }
}
```

## Approach 3: Dynamic Programming with Floyd-Warshall Algorithm

### Solution
```java
// Time Complexity: O(V^3)
// Space Complexity: O(V^2)
import java.util.Arrays;

public class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        int[][] dp = new int[n+1][n+1];
        for (int i = 0; i <= n; i++) {
            Arrays.fill(dp[i], Integer.MAX_VALUE / 2);
            dp[i][i] = 0; // Distance from a node to itself is 0
        }

        // Initialize graph with given times
        for (int[] time : times) {
            dp[time[0]][time[1]] = time[2];
        }

        // Floyd-Warshall algorithm for shortest paths
        for (int t = 1; t <= n; t++) {
            for (int u = 1; u <= n; u++) {
                for (int v = 1; v <= n; v++) {
                    if (dp[u][t] != Integer.MAX_VALUE && dp[t][v] != Integer.MAX_VALUE) {
                        dp[u][v] = Math.min(dp[u][v], dp[u][t] + dp[t][v]);
                    }
                }
            }
        }

        // Calculate the longest shortest path from the start node k
        int maxTime = 0;
        for (int i = 1; i <= n; i++) {
            if (dp[k][i] == Integer.MAX_VALUE) 
                return -1;
            maxTime = Math.max(maxTime, dp[k][i]);
        }

        return maxTime;
    }
}
```

