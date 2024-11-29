# 834. [Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Approach 1: DFS to Calculate Subtree Sizes and Initial Distance

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    private List<List<int>> tree;
    private int[] answer, count;

    public int[] SumOfDistancesInTree(int N, int[][] edges) {
        // Initialize the tree and helper arrays
        tree = new List<List<int>>(N);
        answer = new int[N];
        count = new int[N];

        for (int i = 0; i < N; i++) {
            tree.Add(new List<int>());
        }

        // Build the tree as an adjacency list
        foreach (var edge in edges) {
            tree[edge[0]].Add(edge[1]);
            tree[edge[1]].Add(edge[0]);
        }

        // Post-order DFS to calculate count and initial distances
        PostOrder(0, -1);

        // Pre-order DFS to calculate final answer
        PreOrder(0, -1, N);

        return answer;
    }

    private void PostOrder(int node, int parent) {
        // Initialize count as 1 (itself)
        count[node] = 1;
        foreach (var neighbor in tree[node]) {
            if (neighbor == parent) continue; // Avoid going back to parent
            PostOrder(neighbor, node);
            // Sum up distances in subtree
            count[node] += count[neighbor];
            answer[node] += answer[neighbor] + count[neighbor];
        }
    }

    private void PreOrder(int node, int parent, int N) {
        foreach (var neighbor in tree[node]) {
            if (neighbor == parent) continue; // Avoid going back to parent
            // Calculate using previously computed distances
            answer[neighbor] = answer[node] - count[neighbor] + (N - count[neighbor]);
            PreOrder(neighbor, node, N);
        }
    }
}
```

This approach uses Depth First Search (DFS) to first calculate the sum of distances from the root to all nodes and uses it to compute the distances for the rest by adjusting them based on the tree structure.

