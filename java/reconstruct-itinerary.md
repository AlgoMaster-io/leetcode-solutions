# [Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)

## Approaches
1. [Hierholzer’s Algorithm with DFS (Backtracking)](#approach-1)
2. [Hierholzer’s Algorithm with Iterative DFS (Using Stack)](#approach-2)

---

## Approach 1: Hierholzer’s Algorithm with DFS (Backtracking)

Hierholzer’s algorithm is a method to find an Eulerian path or cycle (a path or cycle that visits every edge exactly once) in a graph. Since the given problem is about reconstructing an itinerary that visits all flights (edges) exactly once, and starts from "JFK", we can use this approach directly.

The idea behind Hierholzer’s algorithm in this context is:
- Start from "JFK" and greedily visit the lexicographically smallest destination city, marking that flight as "used".
- Use Depth-First Search (DFS) to explore each destination and backtrack when there are no further cities to visit from the current city.
- Append the current city to a route list when you can no longer move forward (i.e., dead-end).

### Detailed Steps:
1. Use a priority queue (a min-heap) to keep the adjacency list for each airport. This ensures the lexicographical order in flight selection.
2. Perform a DFS recursively to explore the itinerary starting from "JFK".
3. Append the current airport to the itinerary only when a dead-end is reached and all flights are utilized from the current airport.
4. Reverse the itinerary at the end to get the correct order.

### Code:
```java
import java.util.*;

public class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        Map<String, PriorityQueue<String>> flightMap = new HashMap<>();
        List<String> itinerary = new LinkedList<>();

        // Build the graph using a map of priority queues
        for (List<String> ticket : tickets) {
            flightMap.computeIfAbsent(ticket.get(0), k -> new PriorityQueue<>()).add(ticket.get(1));
        }

        // Start DFS from "JFK"
        dfs("JFK", flightMap, itinerary);

        // Since we add the airport at the "dead-end", reverse the result to get the actual itinerary
        Collections.reverse(itinerary);
        return itinerary;
    }

    private void dfs(String airport, Map<String, PriorityQueue<String>> flightMap, List<String> itinerary) {
        // Retrieve the destinations from the current airport sorted lexicographically
        PriorityQueue<String> destinations = flightMap.get(airport);

        // Visit all flights originating from the current airport
        while (destinations != null && !destinations.isEmpty()) {
            dfs(destinations.poll(), flightMap, itinerary);
        }

        // Add airport to the route when there are no more destinations to visit (dead-end)
        itinerary.add(airport);
    }
}
```

### Time Complexity:
- The algorithm runs in O(E logV) time, where E is the number of edges (tickets) and V is the number of vertices (airports). This is because for each airport, we may sort its list of destinations (inserting each destination into a priority queue which takes logV time).

### Space Complexity:
- The space complexity is O(V + E) as we need to store the graph (connections between airports) and the list of destinations for each airport.

---

## Approach 2: Hierholzer’s Algorithm with Iterative DFS (Using Stack)

Instead of performing the DFS recursively, this approach uses a stack to iteratively perform DFS. The principle remains the same—ensure all edges are visited exactly once, storing the itinerary in a reverse manner and then reversing it at the end.

### Detailed Steps:
1. Utilize a stack to simulate the DFS process.
2. Start with pushing "JFK" onto the stack.
3. While there are nodes to explore until the stack is empty:
   - Look at the top of the stack to get the current airport.
   - If there are outgoing flights from this airport, move to the lexicographically smallest airport using that flight.
   - If no flights left, add this airport to the route and pop from the stack.
4. Reverse the itinerary list to construct the final itinerary correctly.

### Code:
```java
import java.util.*;

public class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        Map<String, PriorityQueue<String>> flightMap = new HashMap<>();
        List<String> itinerary = new LinkedList<>();

        // Build the graph with a priority queue for lexicographic order
        for (List<String> ticket : tickets) {
            flightMap.computeIfAbsent(ticket.get(0), k -> new PriorityQueue<>()).add(ticket.get(1));
        }

        // Use a stack to implement iterative DFS
        Stack<String> stack = new Stack<>();
        stack.push("JFK");

        // Iterate till stack is empty
        while (!stack.isEmpty()) {
            String currAirport = stack.peek();
            // Get the lexicographically smallest destination
            PriorityQueue<String> destinations = flightMap.get(currAirport);
            // Continue this path if there are still remaining flights
            if (destinations != null && !destinations.isEmpty()) {
                stack.push(destinations.poll());
            } else {
                // All destinations are visited, add this airport to itinerary
                itinerary.add(stack.pop());
            }
        }

        // Reverse to get the correct order
        Collections.reverse(itinerary);
        return itinerary;
    }
}
```

### Time Complexity:
- The time complexity for the iterative implementation is similarly O(E logV), given that each airport's priority queue operations require logV time.

### Space Complexity:
- The space complexity is O(V + E) to store each airport’s destinations and stack operations.

These solutions ensure we can construct an Eulerian path for the provided flight details, complying with the lexicographical order constraint.

