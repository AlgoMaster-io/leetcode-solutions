# [Leetcode 815: Bus Routes](https://leetcode.com/problems/bus-routes/)

## Approaches
- [Approach 1: Breadth-First Search (BFS) with Set](#approach-1-breadth-first-search-bfs-with-set)
- [Approach 2: Optimized BFS with Station to Bus Mapping](#approach-2-optimized-bfs-with-station-to-bus-mapping)

## Approach 1: Breadth-First Search (BFS) with Set

### Intuition:
The problem requires finding the shortest path from the source to the target using a series of buses. This can be framed as a graph problem where each bus route is considered as a node, and we aim to find the shortest path between nodes that connect the source to the target.

1. Use BFS to explore each bus route from the source.
2. Keep track of visited routes to avoid cycles.
3. Use a queue to explore all possibilities in layers (buses taken).

### Code:
```typescript
function numBusesToDestination(routes: number[][], source: number, target: number): number {
    if (source === target) return 0;

    const routesCount = routes.length;
    // Map bus stops to the list of buses (routes) that contain them
    const stopToRoutes: Map<number, Set<number>> = new Map();

    for (let i = 0; i < routesCount; i++) {
        for (const stop of routes[i]) {
            if (!stopToRoutes.has(stop)) {
                stopToRoutes.set(stop, new Set());
            }
            stopToRoutes.get(stop)?.add(i);
        }
    }

    const visitedRoutes: Set<number> = new Set();
    const queue: number[] = [];
    let busesTaken = 0;

    // Start BFS from the source
    if (stopToRoutes.has(source)) {
        const startRoutes = stopToRoutes.get(source);
        startRoutes?.forEach(route => {
            queue.push(route);
            visitedRoutes.add(route);
        });
    }

    while (queue.length > 0) {
        busesTaken++;
        const queueLength = queue.length;
        for (let i = 0; i < queueLength; i++) {
            const currentRoute = queue.shift()!;
            for (const stop of routes[currentRoute]) {
                if (stop === target) {
                    return busesTaken;
                }
                const nextRoutes = stopToRoutes.get(stop);
                nextRoutes?.forEach(nextRoute => {
                    if (!visitedRoutes.has(nextRoute)) {
                        visitedRoutes.add(nextRoute);
                        queue.push(nextRoute);
                    }
                });
            }
        }
    }
    return -1;
}
```

### Time Complexity:
- O(N + RS) where N is the number of bus stops, R is the number of bus routes, and S is the average number of stops per bus.

### Space Complexity:
- O(N + RS) due to auxiliary data structures for mapping stops and BFS queue.

## Approach 2: Optimized BFS with Station to Bus Mapping

### Intuition:
Leverage the mapping of each station to the available bus routes right from the beginning. Directly process routes and stations to limit unnecessary iterations and reduce computational overhead.

1. Start from the source bus stop and explore all connected bus routes.
2. Use a set data structure to avoid repeating the processing of the same routes.

### Optimized Code:
```typescript
function numBusesToDestinationOptimized(routes: number[][], source: number, target: number): number {
    if (source === target) return 0;

    const stopToRoutes: Map<number, number[]> = new Map();

    // Map each stop to the list of routes that pass through it
    for (let i = 0; i < routes.length; i++) {
        for (const stop of routes[i]) {
            if (!stopToRoutes.has(stop)) {
                stopToRoutes.set(stop, []);
            }
            stopToRoutes.get(stop)!.push(i);
        }
    }

    const visitedBuses: Set<number> = new Set();
    const visitedStops: Set<number> = new Set();
    let queue: number[] = [source];
    let busesTaken = 0;

    while (queue.length > 0) {
        let nextQueue: number[] = [];
        busesTaken++;
        while (queue.length > 0) {
            const currentStop = queue.shift()!;
            visitedStops.add(currentStop);
            const routesPassing = stopToRoutes.get(currentStop) || [];

            for (const routeNum of routesPassing) {
                if (visitedBuses.has(routeNum)) continue;
                visitedBuses.add(routeNum);

                for (const stop of routes[routeNum]) {
                    if (stop === target) return busesTaken;
                    if (!visitedStops.has(stop)) {
                        nextQueue.push(stop);
                    }
                }
            }
        }
        queue = nextQueue;
    }

    return -1;
}
```

### Time Complexity:
- O(N + RS) similar to the first approach, where we still process each route and stop.

### Space Complexity:
- O(N + R) due to storing stops, routes, and sets for visited entities. 

This optimized version uses direct mapping of stops to bus routes, helping to prune unnecessary iterations when searching for the target.

