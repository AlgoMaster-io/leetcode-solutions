# [Leetcode 815: Bus Routes](https://leetcode.com/problems/bus-routes/)

## Approaches

1. [Breadth-First Search (BFS)](#bfs)
2. [Bidirectional BFS: Optimized Version](#bidirectional-bfs)

---

### 1. Breadth-First Search (BFS)

The idea is to represent the bus routes as a graph where each route is a node, and there is an edge between two nodes if they share at least one common bus stop. We then perform a breadth-first search starting from routes that include the start stop.

#### Intuition

- Convert stops and routes into a graph problem where each route can be seen as a node.
- If two bus routes share a stop, these nodes are connected.
- From each bus route containing the starting stop, perform a BFS to find the target stop.

#### Algorithm

1. Create a mapping from each stop to all routes passing through it.
2. Initialize a queue for BFS with all routes that include the start stop.
3. Use a set to keep track of visited routes to prevent cycles and redundant searches.
4. Iterate through the queue:
   - For each route, check if it contains the target stop.
   - Add all connected, unvisited routes to the queue.
5. If the target stop is found in any route, return the number of buses taken.
6. If the queue is exhausted without finding the target, return -1.

#### Time Complexity

- **Time Complexity:** O(N * S), where N is the number of routes, and S is the maximum number of stops in a route.
- **Space Complexity:** O(N * S) for storing the adjacency list and the visited set.

#### C++ Code

```cpp
#include <vector>
#include <unordered_set>
#include <unordered_map>
#include <queue>

using namespace std;

int numBusesToDestination(vector<vector<int>>& routes, int start, int target) {
    if (start == target) return 0;
    
    unordered_map<int, vector<int>> stop_to_routes;
    for (int i = 0; i < routes.size(); ++i) {
        for (int stop : routes[i]) {
            stop_to_routes[stop].push_back(i);
        }
    }
    
    queue<pair<int, int>> q;  // {current_route, buses_count}
    unordered_set<int> visited;
    
    for (int route : stop_to_routes[start]) {
        q.push({route, 1});
        visited.insert(route);
    }
    
    while (!q.empty()) {
        auto [current_route, buses] = q.front();
        q.pop();
        
        for (int stop : routes[current_route]) {
            if (stop == target) return buses;
            for (int route_next : stop_to_routes[stop]) {
                if (visited.find(route_next) == visited.end()) {
                    q.push({route_next, buses + 1});
                    visited.insert(route_next);
                }
            }
        }
    }
    
    return -1;
}
```

---

### 2. Bidirectional BFS: Optimized Version

In the bidirectional BFS, we initiate search simultaneously from both the start node and the target node, meeting somewhere in the middle. This technique often reduces the number of traversals required, speeding up the search.

#### Intuition

- Instead of searching all routes from start all the way to target, simultaneously search from both ends to find the shortest path quickly.
- This method can sometimes reduce the complexity when the graph diameter is large.

#### Algorithm

1. Initialize two queues for the BFS search from the start and target.
2. Initialize visited sets to avoid revisiting the same nodes.
3. Alternate between exploring from the start and target until both searches meet or queues are exhausted.

#### Time Complexity

- **Time Complexity:** O(N * S) still applies, but in theory, bidirectional search can often improve runtime significantly by reducing practical exploration paths.
- **Space Complexity:** O(N * S), similar reasoning as above but with potentially less exhaustive memory use.

Since the time and space complexities are similar to the above approach, the Bidirectional BFS offers speedup through practical reduction in search paths rather than theoretical bounds improvements.

#### C++ Code

The bidirectional BFS involves interleaving two BFSs progressively towards each other, which tends to be complex to manage and can introduce more overhead if not handled carefully. Often, in practical terms and for simplicity, a well-implemented single direction BFS suffices for competitive programming in this context. However, keep in mind that bidirectional BFS can sometimes be the optimal approach for more constrained graph scenarios.

