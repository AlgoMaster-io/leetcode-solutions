# [Leetcode 1976: Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

## Approaches
1. [Dijkstra's Algorithm with Count Tracking](#approach1)
2. [Improvement using Priority Queue](#approach2)

---

## Approach 1: Dijkstra's Algorithm with Count Tracking

### Intuition
The problem is an extension of the Dijkstra's Algorithm where not only are we interested in the shortest path, but also the number of distinct ways to achieve this shortest path from the starting point to the destination. The main idea is to use Dijkstra's algorithm to find the shortest path and simultaneously count the number of ways to reach each node using that shortest path length.

### Steps
1. Initialize a `dist` array to hold the shortest path estimate for each node, initially set to infinity, except for the start node which is zero.
2. Use a `ways` array to track the number of ways to get to each node with the shortest path. Initialize it such that `ways[start]` is 1.
3. Utilize a priority queue (min-heap) to always process the node with the currently known shortest distance.
4. As each node is processed, update its neighbors:
   - If a shorter path is found, update the distance and reset the ways count.
   - If a path of the same length is found, increment the ways count for the neighbor.
5. The result is the number of ways to reach the destination node with the shortest distance.

### Implementation

```cpp
#include <vector>
#include <queue>
#include <algorithm>
using namespace std;

int countPaths(int n, vector<vector<int>>& roads) {
    const int MOD = 1e9 + 7;
    vector<vector<pair<int, int>>> graph(n);
    
    // Build the graph: adjacency list representation
    for (const auto& road : roads) {
        int u = road[0], v = road[1], w = road[2];
        graph[u].push_back({v, w});
        graph[v].push_back({u, w});
    }

    // Priority queue to apply Dijkstra's, it stores pairs of <cost, node>
    priority_queue<pair<long long, int>, vector<pair<long long, int>>, greater<>> pq;
    vector<long long> dist(n, LLONG_MAX);
    vector<int> ways(n, 0);

    dist[0] = 0;
    ways[0] = 1;
    pq.push({0, 0}); // Start from node 0 with cost 0

    while (!pq.empty()) {
        auto [currentDist, u] = pq.top();
        pq.pop();

        // If the current distance to this node is not optimal, skip processing
        if (currentDist > dist[u]) continue;

        // Relaxation step
        for (const auto& [v, weight] : graph[u]) {
            long long newDist = currentDist + weight;
            if (newDist < dist[v]) { // Found a new shorter path to node v
                dist[v] = newDist;
                ways[v] = ways[u];
                pq.push({newDist, v});
            } else if (newDist == dist[v]) { // Found another shortest path to node v
                ways[v] = (ways[v] + ways[u]) % MOD;
            }
        }
    }

    return ways[n - 1]; // Number of ways to reach the last node
}
```

### Time Complexity
- **O((N + E) log N)**, where \(N\) is the number of nodes and \(E\) is the number of edges. This is due to the priority queue operations and adjacency list traversal for each edge.

### Space Complexity
- **O(N + E)**, required for the graph representation and auxiliary arrays.

---

## Approach 2: Improvement using Priority Queue

This approach doesn't change the logic significantly from Approach 1 but emphasizes handling of the priority queue operations and maintaining cleaner code for edge relaxation. The improvements focus on optimizing memory usage and a clearer handling of state updates but retains the same complexity.

Implementation and complexity remain similar as in Approach 1.

