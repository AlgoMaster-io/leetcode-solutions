# 787. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Approach 1: DFS with Pruning

### Solution
```cpp
// Time Complexity: O(n^k) in the worst case
// Space Complexity: O(n)
#include <vector>
#include <unordered_map>
#include <limits.h>
using namespace std;

class Solution {
public:
    int result = INT_MAX;

    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        // Create adjacency map for graph edges
        unordered_map<int, unordered_map<int, int>> graph;
        for (const auto& flight : flights) {
            graph[flight[0]][flight[1]] = flight[2];
        }

        // Perform DFS
        dfs(graph, src, dst, K + 1, 0); // K + 1 because we count steps, not edges

        return result == INT_MAX ? -1 : result;
    }

private:
    void dfs(unordered_map<int, unordered_map<int, int>>& graph, int node, int dst, int stops, int cost) {
        if (node == dst) {
            result = cost;
            return;
        }

        if (stops == 0) return; // No more stops available

        if (!graph.count(node)) return; // No outgoing flights

        for (const auto& entry : graph[node]) {
            int neighbor = entry.first;
            int price = entry.second;

            // Pruning: proceed only if this path is cheaper than the best found so far
            if (cost + price > result) continue;

            dfs(graph, neighbor, dst, stops - 1, cost + price);
        }
    }
};
```

## Approach 2: Bellman-Ford Algorithm

### Solution
```cpp
// Time Complexity: O(K * E), where E is the number of edges
// Space Complexity: O(n)
#include <vector>
#include <algorithm>
#include <limits.h>
using namespace std;

class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        vector<int> prices(n, INT_MAX);
        prices[src] = 0;

        // Relax the edges up to K+1 times
        for (int i = 0; i <= K; ++i) {
            vector<int> tempPrices = prices;
            for (const auto& flight : flights) {
                int u = flight[0], v = flight[1], price = flight[2];
                if (prices[u] == INT_MAX) continue;

                if (prices[u] + price < tempPrices[v]) {
                    tempPrices[v] = prices[u] + price;
                }
            }
            prices = tempPrices;
        }

        return prices[dst] == INT_MAX ? -1 : prices[dst];
    }
};
```

## Approach 3: Dijkstra's Algorithm with Priority Queue

### Solution
```cpp
// Time Complexity: O(E + VlogV), where E is the number of edges and V is the number of vertices
// Space Complexity: O(n)
#include <vector>
#include <unordered_map>
#include <queue>
#include <limits.h>
using namespace std;

class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        // Create an adjacency list for the graph
        unordered_map<int, vector<pair<int, int>>> graph;
        for (const auto& flight : flights) {
            graph[flight[0]].emplace_back(flight[1], flight[2]);
        }

        // Priority queue will hold entries of (cost, node, stops remaining)
        priority_queue<vector<int>, vector<vector<int>>, greater<vector<int>>> pq;
        pq.push({0, src, K + 1});

        while (!pq.empty()) {
            auto current = pq.top();
            pq.pop();
            int cost = current[0], node = current[1], stopsRemaining = current[2];

            if (node == dst) {
                return cost;
            }

            if (stopsRemaining > 0) {
                if (!graph.count(node)) continue;
                for (const auto& neighbor : graph[node]) {
                    int nextNode = neighbor.first;
                    int priceToNext = neighbor.second;
                    pq.push({cost + priceToNext, nextNode, stopsRemaining - 1});
                }
            }
        }

        return -1; // No valid path found
    }
};
```

