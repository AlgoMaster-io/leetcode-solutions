# 1976. [Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

## Approach 1: Dijkstra's Algorithm with Modification

### Solution
```java
// Time Complexity: O(E log V) where E is the number of edges and V is the number of vertices
// Space Complexity: O(V)
import java.util.*;

public class Solution {
    public int countPaths(int n, int[][] roads) {
        final int MOD = 1_000_000_007;
        List<List<int[]>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }

        // Construct the graph
        for (int[] road : roads) {
            int u = road[0], v = road[1], time = road[2];
            graph.get(u).add(new int[]{v, time});
            graph.get(v).add(new int[]{u, time});
        }

        // Dijkstra's algorithm modifications
        long[] minDist = new long[n];
        long[] ways = new long[n];
        Arrays.fill(minDist, Long.MAX_VALUE);
        ways[0] = 1;
        minDist[0] = 0;

        PriorityQueue<long[]> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a[0]));
        pq.add(new long[]{0, 0}); // {distance, node}

        while (!pq.isEmpty()) {
            long[] current = pq.poll();
            long currentDist = current[0];
            int u = (int) current[1];

            if (currentDist > minDist[u]) continue;

            // Traverse neighbors
            for (int[] neighbor : graph.get(u)) {
                int v = neighbor[0];
                long time = neighbor[1];

                // Relaxation
                if (minDist[u] + time < minDist[v]) {
                    minDist[v] = minDist[u] + time;
                    ways[v] = ways[u];
                    pq.add(new long[]{minDist[v], v});
                } else if (minDist[u] + time == minDist[v]) {
                    ways[v] = (ways[v] + ways[u]) % MOD;
                }
            }
        }

        return (int) ways[n - 1];
    }
}
```

This solution efficiently finds the number of shortest paths from node 0 to node n-1 using a modified version of Dijkstra's algorithm that keeps track of the number of ways to arrive at each node. It uses a priority queue to explore the shortest paths and maintains an array to store the number of ways to reach each node at the shortest distance.

