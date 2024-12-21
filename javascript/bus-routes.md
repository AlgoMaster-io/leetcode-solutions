# [Leetcode 815: Bus Routes](https://leetcode.com/problems/bus-routes/)

## Approaches
- [Approach 1: Breadth-First Search (BFS) Using Queue](#approach-1-breadth-first-search-bfs-using-queue)

---

## Approach 1: Breadth-First Search (BFS) Using Queue

### Intuition:
The problem of finding the shortest path in terms of bus routes can be effectively approached using a Breadth-First Search (BFS) strategy. BFS is optimal for unweighted graphs where each node at the same level is equidistant from the root. By treating each change of a bus route as a level in BFS, we can find the minimum number of bus routes needed.

### Steps:
1. **Graph Representation**:
   - First, transform the list of bus routes into a graph data structure where each bus stop is a node and there are edges between nodes if they are serviced by a particular bus route.

2. **Tracking Visited Stops and Routes**:
   - Use sets to track visited bus stops and bus routes to avoid cycles and redundant explorations.

3. **BFS Traversal**:
   - Begin from the starting node (source stop), enqueueing it to the BFS queue.
   - Use a level counter to keep track of the number of bus routes used.
   - For each bus stop in the queue, consider each bus route that includes this stop. For each route, enqueue all the stops not yet visited.

4. **Handling the Destination**:
   - If at any point the destination bus stop is reached through any bus, return the number of bus routes used as the level count.

5. **Edge Case**:
   - If the source bus stop is the same as the destination, return 0.

### Detailed Code with Comments:
```javascript
var numBusesToDestination = function(routes, source, target) {
    if (source === target) return 0;

    // Construct a mapping from stops to routes; this captures all routes servicing each stop
    const stopToRoutes = new Map();
    for (let routeIndex = 0; routeIndex < routes.length; routeIndex++) {
        for (let stop of routes[routeIndex]) {
            if (!stopToRoutes.has(stop)) {
                stopToRoutes.set(stop, new Set());
            }
            stopToRoutes.get(stop).add(routeIndex);
        }
    }

    // BFS initialization
    let queue = [source];
    let visitedStops = new Set();
    let visitedRoutes = new Set();
    let busesCount = 0; // This will count the number of buses (routes taken)

    // BFS loop
    while (queue.length > 0) {
        busesCount++; // Increment the bus count for each level of BFS
        let queueLength = queue.length;
        
        // Process all stops at the current level
        for (let i = 0; i < queueLength; i++) {
            let currentStop = queue.shift(); // Dequeue the next stop

            // Iterate through all routes passing through the current stop
            for (let route of stopToRoutes.get(currentStop) || []) {
                if (visitedRoutes.has(route)) continue; // Skip already processed routes
                visitedRoutes.add(route); // Mark the route as visited
                
                // Check each stop within the current route
                for (let stop of routes[route]) {
                    if (stop === target) return busesCount; // Target found, return result
                    if (!visitedStops.has(stop)) {
                        visitedStops.add(stop); // Mark stop as visited
                        queue.push(stop); // Enqueue the stop for further exploration
                    }
                }
            }
        }
    }

    // If target is unreachable
    return -1;
};
```

### Complexity Analysis:
- **Time Complexity**: O(N + R^2) where N is the total number of bus stops and R is the number of bus routes. Each route explores other routes that intersect with it.
- **Space Complexity**: O(N + R) with the use of data structures to track stop-to-route mappings and visited states.

This approach using BFS efficiently allows us to find the shortest path in terms of bus routes, leveraging the power of traversing level by level ensuring minimal transfers.

