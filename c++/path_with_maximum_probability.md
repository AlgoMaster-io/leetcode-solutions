# 1514. [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approach 1: Dijkstra's Algorithm using a Priority Queue

### Solution
```cpp
// Time Complexity: O((n + m) * log n), where n is the number of nodes and m is the number of edges
// Space Complexity: O(n + m)
#include <vector>
#include <queue>
#include <utility>
#include <functional>

using namespace std;

class Solution {
public:
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
        vector<vector<pair<int, double>>> graph(n);
        
        // Build adjacency list for the graph
        for (int i = 0; i < edges.size(); ++i) {
            int u = edges[i][0];
            int v = edges[i][1];
            double prob = succProb[i];
            graph[u].emplace_back(v, prob);
            graph[v].emplace_back(u, prob); // Since the graph is undirected
        }

        vector<double> probabilities(n, 0.0); // Store max probability to each node
        probabilities[start] = 1.0; // Max probability to start node is 1

        priority_queue<pair<double, int>> pq;
        pq.emplace(1.0, start);

        // Process nodes with Dijkstra's-like approach
        while (!pq.empty()) {
            auto [currProb, currNode] = pq.top();
            pq.pop();

            // If we reach the end node, return probability
            if (currNode == end) {
                return currProb;
            }

            if (currProb < probabilities[currNode]) {
                continue;
            }

            for (const auto& [nextNode, edgeProb] : graph[currNode]) {
                // Calculate new probability
                double newProb = currProb * edgeProb;
                if (newProb > probabilities[nextNode]) {
                    probabilities[nextNode] = newProb;
                    pq.emplace(newProb, nextNode);
                }
            }
        }

        return probabilities[end]; // Return max probability to reach end node
    }
};
```

This problem is effectively a modification of the shortest path problem or Dijkstra's algorithm by changing distance sums to probabilities multiplied.

