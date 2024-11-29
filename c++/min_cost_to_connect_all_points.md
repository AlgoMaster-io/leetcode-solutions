# 1584. [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

## Approach 1: Prim's Algorithm 

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <vector>
#include <queue>
#include <cmath>
#include <functional>

using namespace std;

class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& points) {
        int n = points.size();
        vector<bool> inMST(n, false);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> minHeap;
        
        minHeap.emplace(0, 0); // (cost, point)
        
        int totalCost = 0;
        int edgesUsed = 0;

        while (edgesUsed < n) {
            auto [cost, u] = minHeap.top();
            minHeap.pop();

            if (inMST[u]) continue; // Skip if already included in the MST

            inMST[u] = true;
            totalCost += cost;
            edgesUsed++;

            for (int v = 0; v < n; ++v) {
                if (!inMST[v]) {
                    int dist = abs(points[u][0] - points[v][0]) + abs(points[u][1] - points[v][1]);
                    minHeap.emplace(dist, v);
                }
            }
        }

        return totalCost;
    }
};
```

## Approach 2: Kruskal's Algorithm with Union-Find

### Solution
```cpp
// Time Complexity: O(n^2 log n)
// Space Complexity: O(n^2)
#include <vector>
#include <queue>
#include <cmath>

using namespace std;

class UnionFind {
private:
    vector<int> parent, rank;

public:
    UnionFind(int n) {
        parent.resize(n);
        rank.resize(n, 1);
        for (int i = 0; i < n; ++i) parent[i] = i;
    }

    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }

    bool unionSets(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);

        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else {
                parent[rootY] = rootX;
                ++rank[rootX];
            }
            return true;
        }

        return false;
    }
};

class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& points) {
        int n = points.size();
        priority_queue<vector<int>, vector<vector<int>>, greater<>> edges;

        // Calculate all possible edges and their costs
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                int cost = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1]);
                edges.push({cost, i, j});
            }
        }

        UnionFind uf(n);
        int totalCost = 0;
        int edgesUsed = 0;

        while (edgesUsed < n - 1) {
            auto edge = edges.top();
            edges.pop();
            
            int cost = edge[0];
            int u = edge[1];
            int v = edge[2];

            if (uf.unionSets(u, v)) {
                totalCost += cost;
                edgesUsed++;
            }
        }

        return totalCost;
    }
};
```

