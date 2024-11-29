# 743. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
```cpp
// Time Complexity: O((V + E) log V)
// Space Complexity: O(V + E)
#include <vector>
#include <unordered_map>
#include <queue>
#include <climits>
#include <algorithm>

using namespace std;

class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        // Create a graph as adjacency list representation
        unordered_map<int, vector<pair<int, int>>> graph;
        for (const auto& time : times) {
            graph[time[0]].emplace_back(time[1], time[2]);
        }

        // Priority Queue to hold nodes as pairs of (time, node)
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        pq.emplace(0, k);

        // Map to track the minimum time to reach each node
        unordered_map<int, int> minTimeToReach;

        // Dijkstra's algorithm
        while (!pq.empty()) {
            int currentTime = pq.top().first;
            int currentNode = pq.top().second;
            pq.pop();

            if (minTimeToReach.count(currentNode)) 
                continue;

            minTimeToReach[currentNode] = currentTime;

            if (graph.count(currentNode)) {
                for (const auto& adj : graph[currentNode]) {
                    int nextNode = adj.first;
                    int travelTime = adj.second;
                    if (!minTimeToReach.count(nextNode)) {
                        pq.emplace(currentTime + travelTime, nextNode);
                    }
                }
            }
        }

        // Determine the maximum time needed
        if (minTimeToReach.size() != n) 
            return -1;

        int maxTime = 0;
        for (const auto& [node, time] : minTimeToReach) {
            maxTime = max(maxTime, time);
        }

        return maxTime;
    }
};
```

## Approach 2: Bellman-Ford Algorithm

### Solution
```cpp
// Time Complexity: O(V * E)
// Space Complexity: O(V)
#include <vector>
#include <climits>
#include <algorithm>

using namespace std;

class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<int> dist(n + 1, INT_MAX);
        dist[k] = 0;

        // Relaxation process for V-1 times
        for (int i = 1; i < n; i++) {
            for (const auto& edge : times) {
                int u = edge[0], v = edge[1], w = edge[2];
                if (dist[u] != INT_MAX && dist[u] + w < dist[v]) {
                    dist[v] = dist[u] + w;
                }
            }
        }

        int maxTime = 0;
        for (int i = 1; i <= n; i++) {
            if (dist[i] == INT_MAX) 
                return -1;
            maxTime = max(maxTime, dist[i]);
        }

        return maxTime;
    }
};
```

## Approach 3: Dynamic Programming with Floyd-Warshall Algorithm

### Solution
```cpp
// Time Complexity: O(V^3)
// Space Complexity: O(V^2)
#include <vector>
#include <climits>
#include <algorithm>

using namespace std;

class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<vector<int>> dp(n+1, vector<int>(n+1, INT_MAX / 2));
        for (int i = 0; i <= n; i++) {
            dp[i][i] = 0; // Distance from a node to itself is 0
        }

        // Initialize graph with given times
        for (const auto& time : times) {
            dp[time[0]][time[1]] = time[2];
        }

        // Floyd-Warshall algorithm for shortest paths
        for (int t = 1; t <= n; t++) {
            for (int u = 1; u <= n; u++) {
                for (int v = 1; v <= n; v++) {
                    if (dp[u][t] != INT_MAX / 2 && dp[t][v] != INT_MAX / 2) {
                        dp[u][v] = min(dp[u][v], dp[u][t] + dp[t][v]);
                    }
                }
            }
        }

        // Calculate the longest shortest path from the start node k
        int maxTime = 0;
        for (int i = 1; i <= n; i++) {
            if (dp[k][i] == INT_MAX / 2) 
                return -1;
            maxTime = max(maxTime, dp[k][i]);
        }

        return maxTime;
    }
};
```

