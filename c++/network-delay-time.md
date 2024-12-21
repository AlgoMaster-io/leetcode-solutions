# [Leetcode 743: Network Delay Time](https://leetcode.com/problems/network-delay-time/)

## Table of Contents
- [Approach 1: Bellman-Ford Algorithm](#approach-1-bellman-ford-algorithm)
- [Approach 2: Dijkstra's Algorithm Using Priority Queue](#approach-2-dijkstras-algorithm-using-priority-queue)
- [Approach 3: Dijkstra's Algorithm Using Min-Heap (Optimized)](#approach-3-dijkstras-algorithm-using-min-heap-optimized)

---

## Approach 1: Bellman-Ford Algorithm

### Intuition
The Bellman-Ford algorithm is used for finding the shortest path from a source to all vertices in a graph with possibly negative weights. In this problem, since weights are non-negative, it can still be applied. It relaxes all the edges V-1 times where V is the number of vertices.

### Steps
1. Initialize the time to infinite for all nodes except the source node, which is set to 0.
2. Relax all edges |V|-1 times, updating the time taken to reach each node.
3. Check for any further relaxation, which implies a negative weight cycle. Since our weights are positive, this is not necessary.
4. Find the maximum time taken amongst all nodes. If any node cannot be reached, return -1.

### C++ Code
```cpp
#include <vector>
#include <limits>
using namespace std;

int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    vector<int> distance(n + 1, numeric_limits<int>::max());
    distance[k] = 0;

    for (int i = 0; i < n - 1; ++i) {
        for (auto& edge : times) {
            int u = edge[0], v = edge[1], w = edge[2];
            if (distance[u] != numeric_limits<int>::max() && distance[u] + w < distance[v]) {
                distance[v] = distance[u] + w;
            }
        }
    }

    int maxTime = 0;
    for (int i = 1; i <= n; ++i) {
        if (distance[i] == numeric_limits<int>::max())
            return -1;
        maxTime = max(maxTime, distance[i]);
    }

    return maxTime;
}
```

### Complexity
- **Time Complexity:** O(VE) where V is the number of vertices and E is the number of edges.
- **Space Complexity:** O(V) to store the distance array.

---

## Approach 2: Dijkstra's Algorithm Using Priority Queue

### Intuition
Dijkstra’s algorithm is efficient for finding the shortest path from a source node to all other nodes in a graph with non-negative weights. The priority queue is used to access the next node with the minimal time efficiently.

### Steps
1. Use a priority queue to keep track of nodes with the shortest cumulative time.
2. Relax all adjacent edges, updating the shortest path estimates.
3. Continue until all nodes are processed. 

### C++ Code
```cpp
#include <vector>
#include <queue>
#include <unordered_map>
using namespace std;

int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    unordered_map<int, vector<pair<int, int>>> graph;
    for (auto& time : times) {
        graph[time[0]].emplace_back(time[1], time[2]);
    }

    vector<int> dist(n + 1, numeric_limits<int>::max());
    dist[k] = 0;

    using P = pair<int, int>;
    priority_queue<P, vector<P>, greater<P>> pq;
    pq.emplace(0, k);

    while (!pq.empty()) {
        auto [curDist, u] = pq.top(); pq.pop();

        if (curDist > dist[u]) continue;
        
        for (auto& [v, time] : graph[u]) {
            if (dist[u] + time < dist[v]) {
                dist[v] = dist[u] + time;
                pq.emplace(dist[v], v);
            }
        }
    }

    int maxTime = *max_element(dist.begin() + 1, dist.end());
    return maxTime == numeric_limits<int>::max() ? -1 : maxTime;
}
```

### Complexity
- **Time Complexity:** O(V + E log V) where V is the number of nodes and E is the number of edges.
- **Space Complexity:** O(V + E) for the priority queue and adjacency list.

---

## Approach 3: Dijkstra's Algorithm Using Min-Heap (Optimized)

### Intuition
An optimized version of Dijkstra's algorithm leverages a min-heap to more efficiently select the node with the smallest tentative distance, thereby optimizing the searching process.

### Steps
1. Build an adjacency list to represent the graph.
2. Maintain a min-heap to process nodes in order of increasing distance from the source.
3. Update the shortest path as each node is dequeued.
4. If all nodes are processed, compute the maximum path length; otherwise, if any node is unreachable, return -1.

### C++ Code
```cpp
#include <vector>
#include <queue>
#include <unordered_map>
using namespace std;

int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    unordered_map<int, vector<pair<int, int>>> graph;
    for (const auto& time : times) {
        graph[time[0]].emplace_back(time[1], time[2]);
    }

    vector<int> dist(n + 1, numeric_limits<int>::max());
    dist[k] = 0;
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> minHeap;
    minHeap.emplace(0, k);

    while (!minHeap.empty()) {
        auto [currentDist, u] = minHeap.top();
        minHeap.pop();
        if (currentDist > dist[u]) continue;

        for (const auto& [v, time] : graph[u]) {
            if (dist[u] + time < dist[v]) {
                dist[v] = dist[u] + time;
                minHeap.emplace(dist[v], v);
            }
        }
    }

    int maxTime = *max_element(dist.begin() + 1, dist.end());
    return maxTime == numeric_limits<int>::max() ? -1 : maxTime;
}
```

### Complexity
- **Time Complexity:** O((E + V) log V) where V is the number of nodes and E is the number of edges.
- **Space Complexity:** O(V + E) for the graph representation and data storage.

