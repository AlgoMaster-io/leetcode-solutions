# [Reconstruct Itinerary - LeetCode](https://leetcode.com/problems/reconstruct-itinerary/)

## Approaches
1. [Hierarchical DFS with Backtracking](#approach-1-hierarchical-dfs-with-backtracking)
2. [Hierarchical DFS with Sorting and Using Heap](#approach-2-hierarchical-dfs-with-sorting-and-using-heap)
3. [Hierholzer's Algorithm for Eulerian Path](#approach-3-hierholzer-algorithm-for-eulerian-path)

### Approach 1: Hierarchical DFS with Backtracking
In this approach, we employ DFS along with backtracking to reconstruct the lexicographically smallest itinerary.

**Intuition:**
- We perform a DFS starting from "JFK", attempting to explore every possible path.
- As we move along the paths, we use backtracking to retract our steps if a path does not result in visiting all tickets.

**Steps:**
1. Use a hashmap to store the destinations in a min priority order for each airport.
2. Perform a DFS starting from "JFK", recursively building the path.
3. Backtrack whenever we hit a dead end or an invalid path.
4. Return the path once all tickets are used.

**Code:**
```typescript
function findItinerary(tickets: string[][]): string[] {
    const flightMap = new Map<string, string[]>();

    // Construct the flight map
    tickets.forEach(([from, to]) => {
        if (!flightMap.has(from)) {
            flightMap.set(from, []);
        }
        flightMap.get(from)!.push(to);
    });

    // Sort the destinations for each airport in lexicographical order
    flightMap.forEach(destinations => destinations.sort());

    const result: string[] = [];
    const visit = (airport: string) => {
        const destinations = flightMap.get(airport);
        // Traverse all destinations starting with the smallest alphabetically
        while (destinations && destinations.length) {
            visit(destinations.shift()!); // Explore further down the tree
        }
        result.push(airport); // Backtracking step
    };

    visit('JFK');
    return result.reverse(); // Since we collect in reverse order
}
```

**Time Complexity:**  \(O(E \log E)\), where \(E\) is the number of flights. Sorting destinations adds \(\log\) complexity.

**Space Complexity:** \(O(V + E)\), where \(V\) is the number of airports and \(E\) the number of flights. This accounts for the recursion stack and the flight map storage.

### Approach 2: Hierarchical DFS with Sorting and Using Heap
This approach also makes use of DFS, but instead of using manual sorting for each node, we can use a priority queue (heap) to dynamically give us the smallest next path.

**Code:**
```typescript
function findItinerary(tickets: string[][]): string[] {
    const flightMap = new Map<string, string[]>();

    tickets.forEach(([from, to]) => {
        if (!flightMap.has(from)) {
            flightMap.set(from, []);
        }
        flightMap.get(from)!.push(to);
    });

    // Function to act like a min heap to manage destinations
    const minHeap = (destinations: string[]) => {
        return destinations.sort((a, b) => a.localeCompare(b)); // Sort lexicographically
    };

    flightMap.forEach((_, key) => {
        flightMap.set(key, minHeap(flightMap.get(key)!));
    });

    const result: string[] = [];
    const visit = (airport: string) => {
        const destinations = flightMap.get(airport);
        while (destinations && destinations.length) {
            visit(destinations.shift()!); // Explore the smallest lexicographical path
        }
        result.push(airport); 
    };

    visit('JFK');
    return result.reverse();
}
```

**Time Complexity:** Same as the first approach, \(O(E \log E)\).

**Space Complexity:** Also \(O(V + E)\).

### Approach 3: Hierholzer's Algorithm for Eulerian Path
This approach exploits the properties of an Eulerian path, where we reuse edges optimally to visit them exactly once.

**Intuition:**
- This method constructs an Eulerian path leveraging Hierholzer’s algorithm.
- As the itinerary represents an Eulerian path where each edge in the graph is traveled exactly once, it allows reconstruction using the smallest lexicographical choice.

**Code:**
```typescript
function findItinerary(tickets: string[][]): string[] {
    const flightMap = new Map<string, string[]>();

    tickets.forEach(([from, to]) => {
        if (!flightMap.has(from)) {
            flightMap.set(from, []);
        }
        flightMap.get(from)!.push(to);
    });

    flightMap.forEach(destinations => destinations.sort()); // Sort destinations

    const result: string[] = [];
    const stack: string[] = ['JFK'];

    while (stack.length > 0) {
        const current = stack[stack.length - 1];
        if (flightMap.has(current) && flightMap.get(current)!.length > 0) {
            stack.push(flightMap.get(current)!.shift()!); // Push next possible destination
        } else {
            result.push(stack.pop()!); // Backtrack
        }
    }

    return result.reverse(); // Result gathered in reverse order
}
```

**Time Complexity:** Also \(O(E \log E)\).

**Space Complexity:** \(O(V + E)\).

Through these approaches, we can correctly reconstruct the itinerary using the given flight tickets, ensuring each ticket is used and following lexicographical order starting from "JFK".

