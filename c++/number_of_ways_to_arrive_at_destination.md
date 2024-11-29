# 1976. [Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

## Approach 1: Dijkstra's Algorithm with Modification

### Solution
```cpp
// Time Complexity: O(E log V) where E is the number of edges and V is the number of vertices
// Space Complexity: O(V)
#include <vector>
#include <queue>
#include <cstring>
#include <functional>
#include <climits>

using namespace std;

class Solution {
public:
    int countPaths(int n, vector<vector<int>>& roads) {
        const int MOD = 1'000'000'007;
        vector<vector<pair<int, int>>> graph(n);

        // Construct the graph
        for (const auto& road : roads) {
            int u = road[0], v = road[1], time = road[2];
            graph[u].emplace_back(v, time);
            graph[v].emplace_back(u, time);
        }

        // Dijkstra's algorithm modifications
        vector<long> minDist(n, LLONG_MAX);
        vector<long> ways(n, 0);
        ways[0] = 1;
        minDist[0] = 0;

        priority_queue<pair<long, int>, vector<pair<long, int>>, greater<>> pq;
        pq.emplace(0, 0); // {distance, node}

        while (!pq.empty()) {
            auto [currentDist, u] = pq.top();
            pq.pop();

            if (currentDist > minDist[u]) continue;

            // Traverse neighbors
            for (const auto& neighbor : graph[u]) {
                int v = neighbor.first;
                long time = neighbor.second;

                // Relaxation
                if (minDist[u] + time < minDist[v]) {
                    minDist[v] = minDist[u] + time;
                    ways[v] = ways[u];
                    pq.emplace(minDist[v], v);
                } else if (minDist[u] + time == minDist[v]) {
                    ways[v] = (ways[v] + ways[u]) % MOD;
                }
            }
        }

        return static_cast<int>(ways[n - 1]);
    }
};
```

This solution efficiently finds the number of shortest paths from node 0 to node n-1 using a modified version of Dijkstra's algorithm that keeps track of the number of ways to arrive at each node. It uses a priority queue to explore the shortest paths and maintains an array to store the number of ways to reach each node at the shortest distance.

