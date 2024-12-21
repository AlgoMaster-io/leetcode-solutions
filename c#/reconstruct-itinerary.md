# [Leetcode 332: Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)

## Table of Contents
- [Approach 1: Backtracking](#approach-1-backtracking)
- [Approach 2: Hierholzer's Algorithm for Eulerian Path](#approach-2-hierholzers-algorithm-for-eulerian-path)

---

## Approach 1: Backtracking

### Intuition
The problem can be reduced to finding an Eulerian path in a directed graph where the nodes are airports and edges are flights. The challenge lies in using each ticket and finding the correct lexicographical order. We will initially attempt to solve the problem using backtracking.

We will start from "JFK" and use a depth-first search (DFS) approach to explore all possible itineraries. The idea is to recursively attempt to build the itinerary and backtrack if we reach a dead end or have completed using all tickets.

### Detailed Comments in Code
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public IList<string> FindItinerary(IList<IList<string>> tickets) {
        // Dictionary to hold all flights originating from each airport (sorted)
        var flights = new Dictionary<string, SortedSet<string>>();
        // Dictionary to store how many tickets exist for each flight
        var ticketCount = new Dictionary<(string from, string to), int>();

        foreach (var ticket in tickets) {
            if (!flights.ContainsKey(ticket[0])) {
                flights[ticket[0]] = new SortedSet<string>();
            }

            flights[ticket[0]].Add(ticket[1]);

            if (!ticketCount.ContainsKey((ticket[0], ticket[1]))) {
                ticketCount[(ticket[0], ticket[1])] = 0;
            }
            ticketCount[(ticket[0], ticket[1])]++;
        }

        var itinerary = new List<string> { "JFK" };

        bool Backtrack(string location) {
            if (itinerary.Count == tickets.Count + 1)
                return true; // All tickets are used

            if (!flights.ContainsKey(location))
                return false; // No more destinations

            foreach (var nextStop in flights[location]) {
                var flight = (location, nextStop);
                if (ticketCount[flight] > 0) {
                    itinerary.Add(nextStop);
                    ticketCount[flight]--;

                    if (Backtrack(nextStop)) {
                        return true; // Found valid itinerary
                    }

                    itinerary.RemoveAt(itinerary.Count - 1); // Backtrack if not valid
                    ticketCount[flight]++;
                }
            }

            return false;
        }

        Backtrack("JFK");
        return itinerary;
    }
}
```

### Time and Space Complexity
- **Time Complexity:** O(E^d), where E is the number of flights and d is the maximum number of flights from a single airport (worst-case scenario depth of recursion).
- **Space Complexity:** O(E), due to storing flights and ticket counts.

---

## Approach 2: Hierholzer's Algorithm for Eulerian Path

### Intuition
An Eulerian path is a path in a graph that visits every edge exactly once. Since the problem guarantees at least one valid path if a solution exists, we can apply Hierholzer's algorithm to construct our itinerary. 

We will use a post-order DFS to generate a valid Eulerian path starting from "JFK". This involves removing an edge immediately after traversing to avoid revisiting the edge, and backtracking only after all outgoing edges are used.

### Detailed Comments in Code
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public IList<string> FindItinerary(IList<IList<string>> tickets) {
        // Use a dictionary with sorted sets for lexicographic order
        var flights = new Dictionary<string, SortedSet<string>>();

        foreach (var ticket in tickets) {
            if (!flights.ContainsKey(ticket[0])) {
                flights[ticket[0]] = new SortedSet<string>();
            }
            flights[ticket[0]].Add(ticket[1]);  // Sorted set automatically maintains order
        }

        var resultStack = new Stack<string>();

        void Visit(string location) {
            while (flights.ContainsKey(location) && flights[location].Count > 0) {
                var nextStop = flights[location].Min;  // Get the smallest (lexicographical) flight
                flights[location].Remove(nextStop);  // Remove used flight
                Visit(nextStop);
            }
            resultStack.Push(location);  // Post-order insert
        }

        Visit("JFK");

        var itinerary = new List<string>();

        while (resultStack.Count > 0) {
            itinerary.Add(resultStack.Pop());
        }

        return itinerary;
    }
}
```

### Time and Space Complexity
- **Time Complexity:** O(E log E), due to the use of the dictionary of sorted sets for storing flights.
- **Space Complexity:** O(E), as we store all flights and due to the size of the recursion stack.

