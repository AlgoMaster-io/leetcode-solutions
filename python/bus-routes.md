# [Leetcode 815: Bus Routes](https://leetcode.com/problems/bus-routes/)

## Approaches:

1. [Breadth-First Search (BFS) Approach](#bfs-approach)
2. [Optimized BFS with Set for buses](#optimized-bfs-with-set-for-buses)

---

### BFS Approach

This problem can be visualized as finding the shortest path in an unweighted graph. Each bus route can be seen as a node and an edge exists between two nodes if they share at least one stop. Using BFS, we can find the shortest path from the source stop to the target stop.

#### Intuition:
- Use BFS because it efficiently finds the shortest path in an unweighted graph.
- Treat each bus route as a node and look for the minimum number of bus routes we need to take.
- Maintain a queue with initial bus routes we can take from the source. Then explore neighboring routes (i.e., bus routes that share stops) to find a path to the target.

#### Detailed Steps:
1. Map each bus stop to all the bus routes that include it.
2. Use a queue for BFS to keep track of the current routes and minimizing the number of buses taken.
3. Use a set to keep track of visited bus routes to avoid reevaluating them.
4. For every route explored, check if the target stop is already, if yes return the count of buses taken thus far.
5. If the queue gets exhausted without hitting the target stop, it means a path isn't available.

```python
from collections import defaultdict, deque

def numBusesToDestination(routes, source, target):
    if source == target:  # If source and target are the same
        return 0

    # Step 1: Build the graph as a mapping from stop to bus routes
    stop_to_buses = defaultdict(set)
    for i, route in enumerate(routes):
        for stop in route:
            stop_to_buses[stop].add(i)

    # Step 2: Initialize the queue for BFS and visited set to avoid re-processing routes
    queue = deque()  # (current_stop, bus_count)
    visited_routes = set()  # To keep track of routes already considered

    # Step 3: Add all initial routes we can take from the source stop
    for bus in stop_to_buses[source]:
        queue.append((bus, 1))
        visited_routes.add(bus)

    # Step 4: Perform BFS to find the shortest path to the target stop
    while queue:
        current_bus, bus_count = queue.popleft()
        
        # Explore all stops in the current bus route
        for stop in routes[current_bus]:
            if stop == target:
                return bus_count  # If target is found, return the bus count

            # Explore neighboring routes from each stop
            for nei_bus in stop_to_buses[stop]:
                if nei_bus not in visited_routes:
                    visited_routes.add(nei_bus)
                    queue.append((nei_bus, bus_count + 1))
    
    return -1  # If the target is never reached

# Time Complexity: O(N + S) where N is the number of routes and S is the total number of stops
# Space Complexity: O(N + S)
```

---

### Optimized BFS with Set for buses

#### Intuition:
To optimize, ensure that stops are not revisited unnecessarily. Additionally, make sure the early exit condition is clear and check if the bus count can be reduced through more intelligent path exploration.

#### Detailed Steps:
- Similar to the first approach, use a dictionary mapping stops to the set of bus routes.
- Employ a `visit` set to prevent revisiting stops, further narrowing down unnecessary checks.

```python
def numBusesToDestination(routes, source, target):
    if source == target:
        return 0

    stop_to_buses = defaultdict(set)
    for bus_index, route in enumerate(routes):
        for stop in route:
            stop_to_buses[stop].add(bus_index)

    queue = deque()
    visited_routes = set()  # Tracks buses taken
    visited_stops = set()  # Tracks stops visited

    # Initialize with all buses accessible from the source
    for bus in stop_to_buses[source]:
        queue.append((bus, 1))
        visited_routes.add(bus)

    while queue:
        current_bus, bus_count = queue.popleft()
        
        for stop in routes[current_bus]:  # Check each stop on current bus line
            if stop == target:
                return bus_count
            
            if stop not in visited_stops:  # Check if the stop has not been visited
                visited_stops.add(stop)
                for bus in stop_to_buses[stop]:  # Check adjacent buses
                    if bus not in visited_routes:
                        visited_routes.add(bus)
                        queue.append((bus, bus_count + 1))
                    
    return -1

# Time Complexity: O(N + S)
# Space Complexity: O(N + S)
```

Both approaches are largely similar but the optimized one maintains additional checks to potentially avoid excess iterations.

