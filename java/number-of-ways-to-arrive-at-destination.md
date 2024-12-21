# [Leetcode Problem 1976: Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

## Table of Contents

1. [Approach 1: Dijkstra's Algorithm with DFS](#approach-1)
2. [Approach 2: Dijkstra's Algorithm with Dynamic Programming](#approach-2)

---

## Approach 1: Dijkstra's Algorithm with DFS <a name="approach-1"></a>

### Intuition

In this approach, we'll use the famous Dijkstra's Algorithm to find the shortest path from the source to the destination. After finding the shortest path, we can perform a DFS from the destination to the source to count all the possible paths matching this shortest path length. This approach is still efficient for the input size allowed by the problem constraints.

### Algorithm

1. Utilize Dijkstra's Algorithm to ensure all minimum weights from the source to all other nodes.
2. Once the shortest path weights are captured, use DFS from the destination to the source counting all possible paths that equal this shortest path weight.
3. Ensure to mod the result by \(10^9 + 7\).

### Code

```java
import java.util.*;

public class Solution {

    private static final int MOD = 1_000_000_007;

    public int countPaths(int n, int[][] roads) {
        List<List<int[]>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }
        for (int[] road : roads) {
            int u = road[0], v = road[1], w = road[2];
            graph.get(u).add(new int[]{v, w});
            graph.get(v).add(new int[]{u, w});
        }

        long[] dist = new long[n];
        Arrays.fill(dist, Long.MAX_VALUE);
        dist[0] = 0;
        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a[1]));
        pq.offer(new int[]{0, 0});

        while (!pq.isEmpty()) {
            int[] pair = pq.poll();
            int node = pair[0];
            long currDist = pair[1];

            if (currDist > dist[node]) continue;

            for (int[] nei : graph.get(node)) {
                int neighbor = nei[0];
                long time = nei[1];

                if (dist[node] + time < dist[neighbor]) {
                    dist[neighbor] = dist[node] + time;
                    pq.offer(new int[]{neighbor, (int) dist[neighbor]});
                }
            }
        }

        return dfs(n - 1, graph, new long[n], dist, dist[n - 1]);
    }

    private int dfs(int node, List<List<int[]>> graph, long[] dp, long[] dist, long targetDist) {
        if (node == 0) return 1;
        if (dp[node] != 0) return (int) dp[node];

        long count = 0;
        for (int[] nei : graph.get(node)) {
            int neighbor = nei[0], time = nei[1];
            if (dist[node] == dist[neighbor] + time) {
                count = (count + dfs(neighbor, graph, dp, dist, targetDist)) % MOD;
            }
        }

        dp[node] = count;
        return (int) count;
    }
}
```

### Complexity Analysis

- **Time Complexity**: \(O(E \log V)\), where \(E\) is the number of roads and \(V\) is the number of cities.
- **Space Complexity**: \(O(V + E)\) due to the adjacency list and other data structures used.

---

## Approach 2: Dijkstra's Algorithm with Dynamic Programming <a name="approach-2"></a>

### Intuition

In this approach, instead of using DFS to count paths after Dijkstra's, we integrate the counting mechanism into 
Dijkstra’s Algorithm itself. This involves maintaining a separate array to count the number of ways to reach each node, updating it as we discover the shortest paths.

### Algorithm

1. Use Dijkstra's Algorithm to compute both the shortest distances and count ways dynamically.
2. Maintain an array to track the number of ways to reach a node when processing its neighbors in the priority queue.
3. If a shorter path found to a node, update the path count to that node as that of the current node.
4. If another equal shortest path found, add the current node's path count to that node's path count.

### Code

```java
import java.util.*;

public class Solution {
    
    private static final int MOD = 1_000_000_007;

    public int countPaths(int n, int[][] roads) {
        List<List<int[]>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }
        for (int[] road : roads) {
            int u = road[0], v = road[1], w = road[2];
            graph.get(u).add(new int[]{v, w});
            graph.get(v).add(new int[]{u, w});
        }

        long[] dist = new long[n];
        int[] ways = new int[n];
        Arrays.fill(dist, Long.MAX_VALUE);
        dist[0] = 0;
        ways[0] = 1;
        
        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a[1]));
        pq.offer(new int[]{0, 0});

        while (!pq.isEmpty()) {
            int[] pair = pq.poll();
            int node = pair[0];
            long currDist = pair[1];

            if (currDist > dist[node]) continue;

            for (int[] nei : graph.get(node)) {
                int neighbor = nei[0];
                long time = nei[1];

                if (dist[node] + time < dist[neighbor]) {
                    dist[neighbor] = dist[node] + time;
                    pq.offer(new int[]{neighbor, (int) dist[neighbor]});
                    ways[neighbor] = ways[node];
                } else if (dist[node] + time == dist[neighbor]) {
                    ways[neighbor] = (ways[neighbor] + ways[node]) % MOD;
                }
            }
        }

        return ways[n - 1];
    }
}
```

### Complexity Analysis

- **Time Complexity**: \(O(E \log V)\), where \(E\) is the number of roads and \(V\) is the number of cities. The performance is similar to standard Dijkstra due to the priority queue.
- **Space Complexity**: \(O(V + E)\), stemming from the adjacency list representation and extra counting array.

Both approaches effectively solve the problem, but Approach 2 focuses more on an integrated solution approach, which can be more efficient in implementation.

