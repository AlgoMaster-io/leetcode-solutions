# [Problem: Leetcode 1514 - Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approaches:
- [Approach 1: Dijkstra’s Algorithm (Modified for Maximum Probability)](#approach-1-dijkstras-algorithm-modified-for-maximum-probability)
- [Approach 2: Bellman-Ford Algorithm](#approach-2-bellman-ford-algorithm)

---

## Approach 1: Dijkstra’s Algorithm (Modified for Maximum Probability)

### Intuition

This problem can be seen as a variant of the shortest path problem where the path with the maximum product of probabilities is desired. A direct application of Dijkstra’s algorithm can be utilized by modifying it to operate on probabilities (as opposed to distances). 

We aim to use a priority queue to always expand the node with the highest probability so far, starting from the source node. This approach ensures that as soon as we "process" a node, the probability associated with reaching it is the maximum possible.

### Algorithm

1. Initialize a max-priority queue with the source node and a probability of 1.0.
2. Maintain an array to store the maximum probability to reach each node.
3. While the priority queue is not empty:
   - Extract the node with the maximum probability.
   - For all its neighbors, update the probability if a higher probability path is found, and push this new path into the queue.
4. Return the probability to reach the destination node.

### Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>

using namespace std;

double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
    vector<vector<pair<int, double>>> graph(n);
    for (int i = 0; i < edges.size(); ++i) {
        int u = edges[i][0], v = edges[i][1];
        double prob = succProb[i];
        graph[u].emplace_back(v, prob);
        graph[v].emplace_back(u, prob);
    }
    
    // The probability to reach each node starting with -1.0 (unvisited)
    vector<double> probabilities(n, -1.0);
    probabilities[start] = 1.0;
    
    // Priority queue to process the node with the highest probability path first
    priority_queue<pair<double, int>> pq;
    pq.emplace(1.0, start);
    
    while (!pq.empty()) {
        double prob = pq.top().first;
        int node = pq.top().second;
        pq.pop();
        
        if (node == end) return prob;
        
        for (auto& neighbor : graph[node]) {
            int neighborNode = neighbor.first;
            double edgeProb = neighbor.second;
            
            // If we find a path with a higher probability
            if (prob * edgeProb > probabilities[neighborNode]) {
                probabilities[neighborNode] = prob * edgeProb;
                pq.emplace(probabilities[neighborNode], neighborNode);
            }
        }
    }
    
    return 0.0;
}
```

### Complexity Analysis

- **Time Complexity:** \(O(E \log V)\), where \(V\) is the number of vertices and \(E\) is the number of edges. The priority queue operations dominate the time complexity.
- **Space Complexity:** \(O(E + V)\), which accounts for the storage used by the graph and the probabilities vector.

---

## Approach 2: Bellman-Ford Algorithm

### Intuition

Bellman-Ford algorithm works well with weighted graphs even when negative weights are present. In this scenario, we can adapt it to maximize the product of the path probabilities, rather than minimizing the sum of path weights.

We iterate over all edges and update the probability for each node the edge connects to.

### Algorithm

1. Initialize the probability of reaching every node to 0 and the probability of the start node to 1.
2. Repeat |V|-1 times:
   - For each edge, calculate the maximum probability path possible and update the probabilities.
3. Return the probability to reach the destination node.

### Code

```cpp
#include <iostream>
#include <vector>

using namespace std;

double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
    vector<double> probabilities(n, 0.0);
    probabilities[start] = 1.0;
    
    // Relaxation process
    for (int i = 0; i < n - 1; ++i) {
        bool updated = false;
        
        for (int j = 0; j < edges.size(); ++j) {
            int u = edges[j][0], v = edges[j][1];
            double prob = succProb[j];
            
            if (probabilities[u] * prob > probabilities[v]) {
                probabilities[v] = probabilities[u] * prob;
                updated = true;
            }
            if (probabilities[v] * prob > probabilities[u]) {
                probabilities[u] = probabilities[v] * prob;
                updated = true;
            }
        }
        
        if (!updated) break; // Stop early if no updates are made
    }
    
    return probabilities[end];
}
```

### Complexity Analysis

- **Time Complexity:** \(O(V \cdot E)\), where \(V\) is the number of vertices and \(E\) is the number of edges.
- **Space Complexity:** \(O(V)\) for storing probabilities for each vertex. 

Both approaches aim to find the maximum probability path, but Dijkstra's (modified for maximum probability) approach is typically more efficient for this type of problem. The Bellman-Ford approach is more general and can handle negative weights, which isn’t necessary for this specific problem scenario.

