# [Leetcode 332: Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)

## Approaches
- [Approach 1: Hierholzer's Algorithm (Eulerian Path in Directed Graph)](#approach-1)
  
---

## Approach 1: Hierholzer's Algorithm (Eulerian Path in Directed Graph)

### Intuition
To reconstruct the itinerary, we need to arrange the given flight tickets such that we visit every airport starting from "JFK" and use all the tickets exactly once. Crucially, if there are multiple valid itineraries, the lexicographically smallest one is desired. This problem is a graph traversal problem where each airport represents nodes and tickets represent edges. The task can be reduced to finding an Eulerian path — a path in a graph that visits every edge exactly once.

Hierholzer’s algorithm is used for finding Eulerian paths or circuits. It involves choosing a path starting from a vertex with a positive degree (here "JFK") and continuing through the vertices via unused edges until it reaches a vertex where no unused edges are available.

### Steps:
1. **Construct the Graph:** 
   - Use an adjacency list to store direct flights from one airport to another, sorted in lexicographical order since we need the smallest itinerary.
   - Use a dictionary where keys are departure airports and values are lists of destination airports.

2. **Perform Depth First Search (DFS):**
   - Starting from "JFK", use DFS to traverse the graph. 
   - Ensure that while exploring the itinerary through a vertex, the destinations are sorted and processed in lexicographic order.
   
3. **Build the Itinerary Backwards:**
   - As you exhaust all paths from a vertex, backtrack and add the node to the itinerary. Since this logic subtracts from the potential paths, it effectively constructs the itinerary in reverse order.
   
4. **Reverse the Result:**
   - Finally, reverse the collected path to get the correct itinerary starting with "JFK".

### Python Code
```python
from collections import defaultdict, deque

def findItinerary(tickets):
    # Step 1: Create graph
    graph = defaultdict(deque)
    for depart, arrive in sorted(tickets):
        graph[depart].append(arrive)
    
    itinerary, stack = [], ['JFK']
    
    # Step 2: Perform DFS
    while stack:
        while graph[stack[-1]]:
            next_city = graph[stack[-1]].popleft()
            stack.append(next_city)
        itinerary.append(stack.pop())
    
    # Step 3: Reverse because we collected the path backwards
    return itinerary[::-1]
```

### Detailed Comments
- **Constructing the Graph:**
  - The `graph` is a dictionary that maps departure airports to a deque of arrival airports. Deques efficiently allow popleft operations, which are pivotal when visiting lexicographically next nodes.

- **Depth First Search:**
  - The `stack` is initialized with 'JFK', the fixed starting point.
  - While traversing from an airport, the smallest lexicographic airport (sorted by default dictionary structure) is always processed.

- **Itinerary Building:**
  - Once we can't travel any further from a city, it's appended to the `itinerary`.
  - This backtracking ensures vertices are appended in reverse visiting order, needing a final reversal of the `itinerary`.

### Time Complexity
- The time complexity is `O(E log V)` where `E` is the number of flights (edges) and `V` is the number of airports (vertices) due to sorting operations on each node's adjacency list.

### Space Complexity
- The space complexity is `O(V + E)` used in storing the graph and additional structures for tracking visits and results.

By employing Hierholzer’s method and leveraging sorting, this approach efficiently ensures that the itinerary formed starts with the provided "JFK" and respects lexicographical order constraints.

