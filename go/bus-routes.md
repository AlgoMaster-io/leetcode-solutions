# [Leetcode 815: Bus Routes](https://leetcode.com/problems/bus-routes/)

## Approaches
- [Breadth First Search (BFS)](#breadth-first-search-bfs)

## Breadth First Search (BFS)

### Intuition:
The problem can be visualized as finding the shortest path in an unweighted graph. The bus stops are the nodes, and a route between any two stops on the same bus is an edge between those nodes. We can simplify this traversal by using BFS, because it naturally finds the shortest path in such scenarios.

Each bus route can be treated as a node itself, and if two routes connect via a common bus stop, there's an edge between these nodes. Thus, the BFS will explore route-to-route connections until it reaches a bus containing the destination stop.

### Explanation:
1. **Build a mapping** (`stop_to_routes`) from each stop to the list of routes that pass through that stop.
2. **Use BFS** to traverse this mapping:
   - Initialize the BFS with all routes that contain the `source` stop.
   - Use a queue to keep track of routes to explore and a visited set to prevent re-processing the same route.
   - For each route under consideration, check if the `target` stop is in the list of stops. If it is, return the count of buses taken.
   - Otherwise, add all unvisited routes connected via this route's stops to the queue and mark them as visited.
3. If the BFS completes without finding the `target`, it means it's unreachable, and return -1.

### Code Implementation:

```go
func numBusesToDestination(routes [][]int, source int, target int) int {
    if source == target {
        return 0
    }
    
    // Maps each stop to the list of routes containing the stop
    stopToRoutes := make(map[int][]int)
    
    for i, route := range routes {
        for _, stop := range route {
            stopToRoutes[stop] = append(stopToRoutes[stop], i)
        }
    }
    
    visitedRoutes := make(map[int]bool)
    queue := []int{} // route indices
    steps := 0
    
    for _, route := range stopToRoutes[source] {
        queue = append(queue, route)
        visitedRoutes[route] = true
    }
    
    // BFS through routes
    for len(queue) > 0 {
        steps++
        nextQueue := []int{}
        
        for _, currentRoute := range queue {
            // check for target in current route
            for _, stop := range routes[currentRoute] {
                if stop == target {
                    return steps
                }
            }
            
            // Expand to other routes via common bus stops
            for _, stop := range routes[currentRoute] {
                for _, nextRoute := range stopToRoutes[stop] {
                    if !visitedRoutes[nextRoute] {
                        visitedRoutes[nextRoute] = true
                        nextQueue = append(nextQueue, nextRoute)
                    }
                }
            }
        }
        
        queue = nextQueue
    }
    
    return -1
}
```

### Complexity Analysis:
- **Time Complexity**: O(N + S), where N is the number of buses (or routes) and S is the total number of stops across all routes. The BFS traverses routes and checks each stop in them.
- **Space Complexity**: O(N + S), due to the `stopToRoutes` mapping and the `visitedRoutes` set, as well as potentially storing all route indices in the queue at any given time.

