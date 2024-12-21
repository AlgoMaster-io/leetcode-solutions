# [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Table of Contents
1. [Naive DFS Approach](#naive-dfs-approach)
2. [Memoization with DFS](#memoization-with-dfs)
3. [Bellman-Ford Algorithm](#bellman-ford-algorithm)
4. [Dijkstra's Algorithm with Priority Queue](#dijkstra-algorithm-with-priority-queue)

---

### Naive DFS Approach

**Intuition**: Start by exploring all possible paths from `src` to `dst` with at most `k` stops using a depth-first search. This helps in understanding the problem spatially but results in a large time due to its exhaustive searching nature.

```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<vector<pair<int, int>>> graph(n);
        for (auto &flight : flights) {
            graph[flight[0]].emplace_back(flight[1], flight[2]);
        }
        
        int result = INT_MAX;
        // DFS function
        function<void(int, int, int, int)> dfs = [&](int node, int cost, int stops, int &minCost) {
            if (node == dst) {
                minCost = min(minCost, cost);
                return;
            }
            
            if (stops > k) return;
            
            for (auto &neighbor : graph[node]) {
                if (cost + neighbor.second >= minCost) continue; // Pruning
                dfs(neighbor.first, cost + neighbor.second, stops + 1, minCost);
            }
        };
        
        dfs(src, 0, 0, result);
        
        return result == INT_MAX ? -1 : result;
    }
};
```

- **Time Complexity**: \(O(n^k)\) - In the worst case, all nodes are connected and we visit all nodes with up to `k` stops.
- **Space Complexity**: \(O(n)\) - For the recursion stack and graph storage.

---

### Memoization with DFS

**Intuition**: Use memoization to store the result of subproblems. This prevents revisiting the same subproblem and significantly reduces the number of calculations.

```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<vector<pair<int, int>>> graph(n);
        for (auto &flight : flights) {
            graph[flight[0]].emplace_back(flight[1], flight[2]);
        }
        
        vector<vector<int>> memo(n, vector<int>(k + 2, -1));
        
        function<int(int, int)> dfs = [&](int node, int stops) -> int {
            if (node == dst) return 0;
            if (stops > k) return INT_MAX;
            if (memo[node][stops] != -1) return memo[node][stops];
            
            int minCost = INT_MAX;
            for (auto &neighbor : graph[node]) {
                int nextCost = dfs(neighbor.first, stops + 1);
                if (nextCost != INT_MAX) {
                    minCost = min(minCost, nextCost + neighbor.second);
                }
            }
            
            return memo[node][stops] = minCost;
        };
        
        int result = dfs(src, 0);
        return result == INT_MAX ? -1 : result;
    }
};
```

- **Time Complexity**: \(O(k \times n^2)\) - Each subproblem is computed once.
- **Space Complexity**: \(O(n \times k)\) - Stores the result of each subproblem in the memoization table.

---

### Bellman-Ford Algorithm

**Intuition**: Utilize dynamic programming to find the shortest path. In each iteration, we update costs for the paths up to `k + 1` times.

```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<int> cost(n, INT_MAX);
        cost[src] = 0;

        // Relaxation for at most k + 1 times
        for (int i = 0; i <= k; ++i) {
            vector<int> temp(cost);
            for (auto &flight : flights) {
                int u = flight[0], v = flight[1], w = flight[2];
                if (cost[u] != INT_MAX) {
                    temp[v] = min(temp[v], cost[u] + w);
                }
            }
            cost = temp;
        }
        
        return cost[dst] == INT_MAX ? -1 : cost[dst];
    }
};
```

- **Time Complexity**: \(O(k \times E)\) - `E` is the number of flights.
- **Space Complexity**: \(O(n)\) - Space for storing costs.

---

### Dijkstra's Algorithm with Priority Queue

**Intuition**: Use Dijkstra's algorithm with a priority queue to efficiently find the minimum cost. Instead of minimizing the total number of stops, we use costs to determine the priority.

```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<vector<pair<int, int>>> graph(n);
        for (auto &flight : flights) {
            graph[flight[0]].emplace_back(flight[1], flight[2]);
        }

        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<tuple<int, int, int>>> pq;
        pq.push({0, src, 0}); // Cost, current node, stops

        while (!pq.empty()) {
            auto [cost, node, stops] = pq.top();
            pq.pop();

            if (node == dst) return cost;
            if (stops > k) continue;

            for (auto &neighbor : graph[node]) {
                pq.push({cost + neighbor.second, neighbor.first, stops + 1});
            }
        }
        
        return -1;
    }
};
```

- **Time Complexity**: \(O(E \log E)\) - Mainly due to priority queue operations.
- **Space Complexity**: \(O(n)\) - For storing graph representation and priority queue.

---

Each approach gradually improves upon the prior, enhancing efficiency by reducing redundant computations and utilizing advanced data structures to expedite finding a solution.

