# [Leetcode 332: Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)

## Approaches:
1. [Backtracking Approach](#backtracking-approach)
2. [Hierholzer's Algorithm for Eulerian Path](#hierholzers-algorithm-for-eulerian-path)

### Backtracking Approach

The problem of reconstructing an itinerary can be approached using backtracking. The main idea is to explore all possible paths starting from "JFK" and choose the path with the smallest lexical order.

#### Intuition:
- Sort each list of destinations to ensure that we try smaller lexical airports first.
- Use a Depth First Search (DFS) approach to explore each path, backtracking when a dead end is reached.
- Maintain an itinerary list, and once a route is finalized, update the itinerary.

#### Steps:
1. Create a graph using a hashmap where each key is an airport and each value is a list of destinations.
2. Sort the destinations to ensure lexical order.
3. Perform DFS starting from "JFK".
4. Once a complete path that uses all tickets is found, backtrack and adjust the itinerary to reflect the correct order.

```javascript
const findItinerary = (tickets) => {
    // Create a graph as an adjacency list
    const graph = {};
    tickets.forEach(([from, to]) => {
        if (!graph[from]) graph[from] = [];
        graph[from].push(to);
    });

    // Sort the destinations to ensure lexical order
    for (let from in graph) {
        graph[from].sort();
    }

    const itinerary = [];
    
    const dfs = (airport) => {
        const destinations = graph[airport];
        while (destinations && destinations.length > 0) {
            const nextDestination = destinations.shift(); // Choose the next lexical destination
            dfs(nextDestination); // Explore the path
        }
        itinerary.push(airport); // Add the airport to itinerary when returning from recursion
    };

    dfs("JFK");
    return itinerary.reverse(); // The itinerary will be built in reverse order
};

// Time Complexity: O(E log E), where E is the number of edges.
// Space Complexity: O(V + E), where V is the number of vertices.
```

### Hierholzer's Algorithm for Eulerian Path

An optimal solution can be derived using Hierholzer's algorithm, which finds an Eulerian path in a graph. This is because each ticket can be thought of as an edge in a graph, and finding the itinerary is equivalent to finding an Eulerian path that visits each edge exactly once.

#### Intuition:
- The graph's structure as described suits a directed graph with an Eulerian path.
- Start from "JFK" and always choose the smallest lexical order ticket first.
- As you progress, this path will create a valid Eulerian path that uses all tickets exactly once.

#### Steps:
1. Create an adjacency list from the tickets.
2. Sort each destination list to ensure lexical order.
3. Use DFS to traverse the graph. When no further tickets are available from a node, backtrack and record the node in the itinerary.
4. Reverse the recorded order to get the final itinerary.

```javascript
const findItineraryEulerian = (tickets) => {
    const graph = {};
    tickets.forEach(([from, to]) => {
        if (!graph[from]) graph[from] = [];
        graph[from].push(to);
    });

    for (let from in graph) {
        graph[from].sort((a, b) => a.localeCompare(b));
    }

    const itinerary = [];
    
    const visit = (airport) => {
        const destinations = graph[airport];
        while (destinations && destinations.length > 0) {
            visit(destinations.shift()); // Visit in lexical order
        }
        itinerary.push(airport); // This happens once all destinations are visited
    };

    visit("JFK");
    return itinerary.reverse(); // Reverse to get the correct order
};

// Time Complexity: O(E log E), where E is the number of edges.
// Space Complexity: O(V + E), where V is the number of vertices.
```

These solutions prioritize step-by-step logic and meticulous construction of the itinerary using graph traversal principles. Each method ensures all paths are explored while aiming to build the itinerary in the smallest lexical order possible when multiple options are present.

