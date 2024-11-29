# 1514. [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approach 1: Dijkstra's Algorithm using a Priority Queue

### Solution
```java
// Time Complexity: O((n + m) * log n), where n is the number of nodes and m is the number of edges
// Space Complexity: O(n + m)
import java.util.*;

public class Solution {
    public double maxProbability(int n, int[][] edges, double[] succProb, int start, int end) {
        List<List<Node>> graph = new ArrayList<>();
        
        // Initialize graph
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }

        // Build adjacency list for the graph
        for (int i = 0; i < edges.length; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            double prob = succProb[i];
            graph.get(u).add(new Node(v, prob));
            graph.get(v).add(new Node(u, prob)); // Since the graph is undirected
        }

        double[] probabilities = new double[n]; // Store max probability to each node
        probabilities[start] = 1.0; // Max probability to start node is 1

        PriorityQueue<Node> pq = new PriorityQueue<>((a, b) -> Double.compare(b.probability, a.probability));
        pq.offer(new Node(start, 1.0));

        // Process nodes with Dijkstra's algorithm
        while (!pq.isEmpty()) {
            Node current = pq.poll();
            int currNode = current.node;
            double currProb = current.probability;

            // If we reach the end node, return probability
            if (currNode == end) {
                return currProb;
            }

            if (currProb < probabilities[currNode]) {
                continue;
            }

            for (Node neighbor : graph.get(currNode)) {
                int nextNode = neighbor.node;
                double edgeProb = neighbor.probability;

                // Calculate new probability
                double newProb = currProb * edgeProb;
                if (newProb > probabilities[nextNode]) {
                    probabilities[nextNode] = newProb;
                    pq.offer(new Node(nextNode, newProb));
                }
            }
        }

        return probabilities[end]; // Return max probability to reach end node
    }

    private static class Node {
        int node;
        double probability;

        Node(int node, double probability) {
            this.node = node;
            this.probability = probability;
        }
    }
}
```

This problem is effectively a modification of the shortest path problem or Dijkstra's algorithm by changing distance sums to probabilities multiplied.

