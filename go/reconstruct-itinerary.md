## [332. Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)

### Approaches
- [Backtracking with Hierholzer's Algorithm](#backtracking-with-hierholzers-algorithm)

---

### Backtracking with Hierholzer's Algorithm

#### Intuition

To solve the problem of finding an itinerary that uses all the given tickets exactly once, starting from "JFK", and that is lexicographically smallest, we can utilize a combination of graph theory and backtracking. Specifically, Hierholzer's algorithm - which is used to find Eulerian paths in a directed graph - is applicable here. The key is to treat each airport as a node and each ticket as a directed edge.

1. **Graph Construction**: Construct a graph with airports as nodes and directed edges representing the tickets. Sort the destinations for each departure point lexicographically to manage the hierarchy in our Eulerian path.

2. **Depth-First Search (DFS) Backtracking**: Using DFS, we traverse the graph, always choosing the smallest available lexical airport (because of sorting). Hierholzer's algorithm helps us construct the path, ensuring all edges (tickets) are used once.

3. **Eulerian Path Construction**: The trick is to append the nodes to the path in reverse order - this is characteristic of Hierholzer's approach for Eulerian circuits and paths. The final path is created by reversing this list.

This method efficiently constructs the path with minimal additional memory usage since we maintain a single path and utilize the input graph for node exploration.

#### Time Complexity
- **Time**: O(E log E), where E is the number of tickets. This arises from sorting the destinations and visiting each edge once.
- **Space**: O(V + E), where V is the number of airports and E is the number of tickets, required for storing the graph representation.

#### Implementation

```go
func findItinerary(tickets [][]string) []string {
    // Map to graph nodes (airports) with a list of destinations
    graph := make(map[string][]string)
    
    // Build the graph where each node points to a sorted list of its neighbors
    for _, ticket := range tickets {
        graph[ticket[0]] = append(graph[ticket[0]], ticket[1])
    }
    
    // Sort the destinations for each airport in lexical order
    for key := range graph {
        sort.Strings(graph[key])
    }
    
    var result []string
    
    // Define DFS function utilizing backtracking to build itinerary
    var dfs func(string)
    dfs = func(airport string) {
        // Visit each destination from current airport, lexicographic minimal first
        for len(graph[airport]) > 0 {
            // Remove the destination as we "use" a ticket
            destination := graph[airport][0]
            graph[airport] = graph[airport][1:]
            dfs(destination)
        }
        // Add airport to itinerary in reverse order
        result = append(result, airport)
    }
    
    // Start our itinerary from "JFK"
    dfs("JFK")
    
    // The result array holds the reverse of the itinerary we want
    // Reverse to get the final itinerary
    for i, j := 0, len(result)-1; i < j; i, j = i+1, j-1 {
        result[i], result[j] = result[j], result[i]
    }
    
    return result
}
```

In this implementation, we leverage a sorted adjacency list and a recursive DFS to ensure we visit the smallest lexicographical destination at each step, guaranteeing the smallest lexicographical order for the itinerary. After completing the DFS, a reversal of the resultant path produces the desired itinerary order.

