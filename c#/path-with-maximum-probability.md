# [Leetcode 1514: Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approaches
- [Approach 1: Dijkstra's Algorithm](#approach-1-dijkstras-algorithm)

---

### Approach 1: Dijkstra's Algorithm

#### Intuition:
The problem of finding the path with the maximum probability can be visualized as a variant of the shortest path problem where instead of minimizing the path cost, we want to maximize the product of edge probabilities. A natural choice for this problem is Dijkstra's Algorithm, which is typically used to find the shortest path in a graph. Here, we'll adapt it to maximize probabilities.

The key idea is to use a priority queue (max-heap) to always expand the path with the highest probability. Since Dijkstra focuses on the shortest path by expanding the lowest cost edges first, in our approach, we need to focus on the highest probability by always expanding paths with the highest probability stored in a max-heap.

#### Steps:
1. **Graph Representation:** 
   - Represent the graph using an adjacency list where each node points to a list of tuples containing the neighboring node and the probability weight of the edge.
   
2. **Max-Heap Initialization:**
   - Use a priority queue (max-heap) to keep track of the maximum probability path we have encountered as we traverse the graph.

3. **Probability Table:**
   - Create an array to store the maximum probability of reaching each node. Initialize the start node with probability 1 (as it's 100% probable to be at the starting location).
   
4. **Dijkstra's Execution:**
   - Start with the source node with probability 1 and attempt to "dijkstra" to the target node.
   - At every step, extract the maximum probability path from the heap, and explore its neighbors.
   - Update the neighbor's probability if passing through the current node gives a higher probability.
   - Push the updated paths onto the heap for further exploration.

5. **Exit Condition:**
   - If the target node is dequeued from the heap, the accompanying probability is the maximum. Return as the solution.

#### Algorithm:
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    
    public double MaxProbability(int n, int[][] edges, double[] succProb, int start, int end) {
        // Step 1: Build adjacency list
        var adjacencyList = new Dictionary<int, List<(int, double)>>();

        for (int i = 0; i < edges.Length; i++) {
            int u = edges[i][0], v = edges[i][1];
            double probability = succProb[i];

            if (!adjacencyList.ContainsKey(u)) {
                adjacencyList[u] = new List<(int, double)>();
            }
            if (!adjacencyList.ContainsKey(v)) {
                adjacencyList[v] = new List<(int, double)>();
            }

            adjacencyList[u].Add((v, probability));
            adjacencyList[v].Add((u, probability));
        }

        // Step 2: Probability table and max-heap (priority queue)
        var maxProb = new double[n];
        maxProb[start] = 1;

        var maxHeap = new SortedSet<(double, int)>(Comparer<(double, int)>.Create((a, b) => {
            // Prioritize larger probabilities, if equal, prioritize by smaller node index (to avoid duplicates)
            return a.Item1 == b.Item1 ? a.Item2.CompareTo(b.Item2) : b.Item1.CompareTo(a.Item1);
        }));

        maxHeap.Add((1, start));  // Start at 'start' node with 100% probability

        // Step 3: Dijkstra-like process based on max probability
        while (maxHeap.Count > 0) {
            var (probability, node) = maxHeap.Max;
            maxHeap.Remove(maxHeap.Max);

            // If we've reached the end node, return the max probability
            if (node == end) {
                return probability;
            }
            
            if (!adjacencyList.ContainsKey(node)) continue;

            foreach (var (neighbor, edgeProb) in adjacencyList[node]) {
                double newProb = probability * edgeProb;
                if (newProb > maxProb[neighbor]) {
                    maxProb[neighbor] = newProb;
                    maxHeap.Add((newProb, neighbor));   // Push new path with updated probability
                }
            }
        }

        // Step 4: If 'end' node never reached, the probability is 0
        return 0.0;
    }
}
```

#### Time Complexity:
- **O(E log V):** Where E is the number of edges and V is the number of vertices. This accounts for the heap operations.

#### Space Complexity:
- **O(V + E):** To hold the adjacency list and other constructs.

This approach efficiently solves the problem by leveraging a modified Dijkstra's algorithm to work with probabilities. By maintaining a priority queue, we ensure that we always expand the path with the current maximum potential, eventually leading us to the target node through the path with the highest product of probabilities.

