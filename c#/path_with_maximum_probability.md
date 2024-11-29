# 1514. [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approach 1: Dijkstra's Algorithm using a Priority Queue

### Solution
csharp
```csharp
// Time Complexity: O((n + m) * log n), where n is the number of nodes and m is the number of edges
// Space Complexity: O(n + m)
using System;
using System.Collections.Generic;
using System.Linq;

public class Solution {
    public double MaxProbability(int n, int[][] edges, double[] succProb, int start, int end) {
        List<List<Node>> graph = new List<List<Node>>();

        // Initialize graph
        for (int i = 0; i < n; i++) {
            graph.Add(new List<Node>());
        }

        // Build adjacency list for the graph
        for (int i = 0; i < edges.Length; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            double prob = succProb[i];
            graph[u].Add(new Node(v, prob));
            graph[v].Add(new Node(u, prob)); // Since the graph is undirected
        }

        double[] probabilities = new double[n]; // Store max probability to each node
        probabilities[start] = 1.0; // Max probability to start node is 1

        var pq = new SortedSet<Node>(Comparer<Node>.Create((a, b) => {
            int cmp = b.Probability.CompareTo(a.Probability);
            return cmp != 0 ? cmp : a.Node.CompareTo(b.Node);
        }));
        pq.Add(new Node(start, 1.0));

        // Process nodes with Dijkstra's algorithm
        while (pq.Count > 0) {
            Node current = pq.Min;
            pq.Remove(current);
            
            int currNode = current.Node;
            double currProb = current.Probability;

            // If we reach the end node, return probability
            if (currNode == end) {
                return currProb;
            }

            if (currProb < probabilities[currNode]) {
                continue;
            }

            foreach (Node neighbor in graph[currNode]) {
                int nextNode = neighbor.Node;
                double edgeProb = neighbor.Probability;

                // Calculate new probability
                double newProb = currProb * edgeProb;
                if (newProb > probabilities[nextNode]) {
                    probabilities[nextNode] = newProb;
                    pq.Add(new Node(nextNode, newProb));
                }
            }
        }

        return probabilities[end]; // Return max probability to reach end node
    }

    private class Node {
        public int Node { get; }
        public double Probability { get; }

        public Node(int node, double probability) {
            this.Node = node;
            this.Probability = probability;
        }
    }
}
```

This problem is effectively a modification of the shortest path problem or Dijkstra's algorithm by changing distance sums to probabilities multiplied.

