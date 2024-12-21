# [Reconstruct Itinerary - LeetCode Problem 332](https://leetcode.com/problems/reconstruct-itinerary/)

## Approaches:
- [Approach 1: Backtracking with Lexicographical Order](#approach-1-backtracking-with-lexicographical-order)
- [Approach 2: Hierholzer's Algorithm](#approach-2-hierholzers-algorithm)

## Approach 1: Backtracking with Lexicographical Order

### Intuition:
The problem can be thought of as finding a route (path) that visits all the flights / tickets exactly once. This is essentially finding an Eulerian path in a directed graph. The naive approach is using backtracking - we attempt to construct the itinerary by trying to use each ticket starting from "JFK". 

1. **Graph Representation:** Use an adjacency list where each airport maps to a list of possible destinations sorted in lexicographical order. This helps to ensure lexical priority.
  
2. **Backtracking Process:** Start from "JFK". For each destination from the current airport, mark it as used and recursively attempt to construct the rest of the itinerary. Backtrack if you reach a point where you can’t proceed further.

3. **Check Completion:** The constructed itinerary must include all tickets used once.

### Code:

```cpp
#include <vector>
#include <string>
#include <unordered_map>
#include <set>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        // Graph adjacency list: Each node maps to a multiset of destinations
        unordered_map<string, multiset<string>> targets;
        
        // Build the graph
        for (const auto& ticket : tickets) {
            targets[ticket[0]].insert(ticket[1]);
        }
        
        vector<string> route;
        visit("JFK", targets, route);
        // The result is built in reverse order, so reverse it
        return vector<string>(route.rbegin(), route.rend());
    }
    
private:
    void visit(string airport, unordered_map<string, multiset<string>>& targets, vector<string>& route) {
        // Explore all outgoing flights from the current airport
        while (!targets[airport].empty()) {
            // Pick the smallest lexical order destination
            auto next = *targets[airport].begin();
            targets[airport].erase(targets[airport].begin());
            visit(next, targets, route);
        }
        // This airport is finished, add it to the route
        route.push_back(airport);
    }
};
```

### Complexity:
- **Time Complexity:** O(E log E), where E is the number of flights (tickets), due to using multiset for sorting destinations.
- **Space Complexity:** O(V + E), where V is the number of unique airports.

## Approach 2: Hierholzer's Algorithm

### Intuition:
The Eulerian path can also be effectively found using Hierholzer’s algorithm. This method constructs the itinerary by completely exhausting paths first before backtracking, ensuring the correct eulerian path order in reverse.

1. **Graph Representation:** As in previous approach, using adjacency list with destinations sorted lexicographically for consistency with problem's requirements.

2. **Hierholzer's Process:** Start the tour from “JFK”. While there’s an outbound flight, take the smallest and proceed. If you hit a dead-end (i.e., no further outbound flights from the current airport), backtrack and add the current airport to the itinerary.

3. **Build Itinerary:** The constructed itinerary will be in the reverse order of the Eulerian path. Reverse it before returning.

### Code:

```cpp
#include <vector>
#include <string>
#include <unordered_map>
#include <stack>
#include <set>

using namespace std;

class Solution {
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        // Graph adjacency list: Each node maps to a multiset of destinations
        unordered_map<string, multiset<string>> graph;
        
        // Construct the graph, ensuring lexical order with multiset
        for (auto& ticket : tickets) {
            graph[ticket[0]].insert(ticket[1]);
        }
        
        vector<string> itinerary;
        stack<string> dfs_stack;
        dfs_stack.push("JFK");
        
        while (!dfs_stack.empty()) {
            string current = dfs_stack.top();
            
            // If there are no more outgoing flights
            if (graph[current].empty()) {
                itinerary.push_back(current);
                dfs_stack.pop();
            } else {
                // Take the smallest lexical order outgoing flight and push it
                auto next = *(graph[current].begin());
                graph[current].erase(graph[current].begin());
                dfs_stack.push(next);
            }
        }
        
        // Reverse to get the correct order
        reverse(itinerary.begin(), itinerary.end());
        return itinerary;
    }
};
```

### Complexity:
- **Time Complexity:** O(E log E), where E is the number of flights (tickets), due to the multiset operations.
- **Space Complexity:** O(V + E). 

Both approaches effectively solve the Eulerian path problem by ensuring all edges (flights) are used exactly once in the path reconstruction.

