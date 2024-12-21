# [Leetcode 815: Bus Routes](https://leetcode.com/problems/bus-routes/)

This problem asks for the minimum number of buses you must take to travel from a start stop to a destination stop. Here's an outline of various approaches to solve this problem:

- [Approach 1: Brute Force BFS](#approach-1-brute-force-bfs)
- [Approach 2: BFS with Optimizations](#approach-2-bfs-with-optimizations)

---

## Approach 1: Brute Force BFS

### Intuition
We can represent the bus stop network as a graph problem where each bus stop is a node, and each bus route represents a direct connection between its stops. Given this graph-like scenario, we can use Breadth-First Search (BFS) to explore all possible routes from the start stop to the target stop.

### Steps
1. Build a graph where nodes are bus stops and edges represent buses that connect two stops directly.
2. Use BFS starting from the start stop to explore all possible connections until the destination is reached.
3. Use a queue to manage the BFS and a set to mark stops as visited.

### Code

```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public int NumBusesToDestination(int[][] routes, int source, int target) {
        if (source == target) return 0;
        
        // Bus stop to bus routes mapping
        Dictionary<int, List<int>> stopToRouteMap = new Dictionary<int, List<int>>();
        
        for (int i = 0; i < routes.Length; i++) {
            foreach (int stop in routes[i]) {
                if (!stopToRouteMap.ContainsKey(stop)) {
                    stopToRouteMap.Add(stop, new List<int>());
                }
                stopToRouteMap[stop].Add(i);
            }
        }
        
        // BFS setup
        Queue<int> queue = new Queue<int>();
        HashSet<int> visitedStops = new HashSet<int>(); // Track visited stops
        HashSet<int> visitedBuses = new HashSet<int>(); // Track taken buses
        
        queue.Enqueue(source);
        visitedStops.Add(source);
        
        int buses = 0;
        
        while (queue.Count > 0) {
            int size = queue.Count;
            buses++;
            
            for (int i = 0; i < size; i++) {
                int currentStop = queue.Dequeue();
                
                foreach (int route in stopToRouteMap[currentStop]) {
                    if (!visitedBuses.Contains(route)) {
                        visitedBuses.Add(route);
                        foreach (int nextStop in routes[route]) {
                            if (nextStop == target) return buses;
                            if (!visitedStops.Contains(nextStop)) {
                                visitedStops.Add(nextStop);
                                queue.Enqueue(nextStop);
                            }
                        }
                    }
                }
            }
        }
        
        return -1;
    }
}
```

### Complexity
- **Time Complexity**: O(N^2 * M), where N is the number of routes and M is the number of stops per route. In the worst case, each stop could be connected to each route.
- **Space Complexity**: O(N*M) for storing stop-to-route mappings and visited stops and routes.

---

## Approach 2: BFS with Optimizations

### Intuition
To improve the efficiency, a slight modification can be applied to ensure we don't revisit the same bus routes, which are already considered.

### Steps
1. Similar to Approach 1, but we now maintain a separate set for marking routes as visited.
2. The logic of BFS remains the same; just reduces redundant computation.

### Code

```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public int NumBusesToDestination(int[][] routes, int source, int target) {
        if (source == target) return 0;
        
        Dictionary<int, List<int>> stopToRouteMap = new Dictionary<int, List<int>>();
        
        // Fill bus stop to bus routes map
        for (int i = 0; i < routes.Length; i++) {
            foreach (int stop in routes[i]) {
                if (!stopToRouteMap.ContainsKey(stop)) {
                    stopToRouteMap.Add(stop, new List<int>());
                }
                stopToRouteMap[stop].Add(i);
            }
        }
        
        Queue<int> queue = new Queue<int>();
        HashSet<int> visitedStops = new HashSet<int>();
        HashSet<int> visitedRoutes = new HashSet<int>();
        
        queue.Enqueue(source);
        visitedStops.Add(source);
        
        int buses = 0;
        
        while (queue.Count > 0) {
            int size = queue.Count;
            buses++;
            
            for (int i = 0; i < size; i++) {
                int currentStop = queue.Dequeue();
                
                foreach (int route in stopToRouteMap[currentStop]) {
                    if (!visitedRoutes.Contains(route)) {
                        visitedRoutes.Add(route);
                        
                        foreach (int nextStop in routes[route]) {
                            if (nextStop == target) return buses;
                            if (!visitedStops.Contains(nextStop)) {
                                visitedStops.Add(nextStop);
                                queue.Enqueue(nextStop);
                            }
                        }
                    }
                }
            }
        }
        
        return -1;
    }
}
```

### Complexity
- **Time Complexity**: O(N * M^2), slightly improved by reducing the number of route re-examinations.
- **Space Complexity**: O(N*M).

This solution leverages BFS more effectively, also utilizing additional data structures efficiently.

