# [Leetcode 815: Bus Routes](https://leetcode.com/problems/bus-routes/)

## Approaches
- [Approach 1: Breadth-First Search (BFS)](#approach-1-breadth-first-search-bfs)

---

## Approach 1: Breadth-First Search (BFS)

### Intuition
This problem can be visualized as finding the shortest path in an unweighted graph. Each route can be considered as a node, and there is an edge between two nodes if they share at least one bus stop. The task is to determine the minimum number of buses needed to travel from the `source` to the `target`. Here’s the breakdown:

1. **Graph Representation**: Use each route as a node. Routes are connected if they share a common bus stop.
2. **BFS Traversal**: Utilize BFS to ensure the minimum transfer path is found.
3. **Starting Points**: Begin from all the routes that contain the `source`.
4. **Destination Condition**: Stop when any route containing the `target` is reached in the BFS traversal.

### Solution
```java
import java.util.*;

public class Solution {
    public int numBusesToDestination(int[][] routes, int source, int target) {
        if (source == target) return 0;

        // Map to track which bus routes are available for each stop
        Map<Integer, List<Integer>> stopToRoutesMap = new HashMap<>();
        
        // Fill the map with stop to routes associations
        for (int i = 0; i < routes.length; i++) {
            for (int stop : routes[i]) {
                if (!stopToRoutesMap.containsKey(stop)) {
                    stopToRoutesMap.put(stop, new ArrayList<>());
                }
                stopToRoutesMap.get(stop).add(i);
            }
        }

        // Use a queue for BFS, each entry is a pair of route index and buses taken so far
        Queue<int[]> queue = new LinkedList<>();
        // Track visited routes to avoid cycles
        boolean[] visited = new boolean[routes.length];

        // Add all routes starting with the source stop
        for (int route : stopToRoutesMap.getOrDefault(source, new ArrayList<>())) {
            queue.offer(new int[]{route, 1});
            visited[route] = true;
        }

        // Begin BFS
        while (!queue.isEmpty()) {
            int[] curr = queue.poll();
            int routeIndex = curr[0];
            int busesTaken = curr[1];

            // Inspect each stop in current route
            for (int stop : routes[routeIndex]) {
                // Check if we've reached the target
                if (stop == target) return busesTaken;

                // Check all neighboring routes of the current stop
                for (int nextRoute : stopToRoutesMap.getOrDefault(stop, new ArrayList<>())) {
                    if (!visited[nextRoute]) {
                        queue.offer(new int[]{nextRoute, busesTaken + 1});
                        visited[nextRoute] = true;
                    }
                }
            }
        }

        return -1; // Target is unreachable
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(N + S) where N is the total number of bus routes and S is the total number of bus stops. Each stop to route association is examined once.
- **Space Complexity**: O(N + S). We store mappings for each stop to their corresponding routes and a visited list for routes.

This approach efficiently finds the minimum bus transfers using BFS, leveraging graph traversal techniques suitable for unweighted graphs.

