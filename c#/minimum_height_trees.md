# 310. [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

## Approach 1: Breadth-First Search (BFS) with Degree Tracking

### Solution
java
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.*;

public class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if (n == 1) return Collections.singletonList(0);
        
        List<Set<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) adj.add(new HashSet<>());
        for (int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }
        
        List<Integer> leaves = new ArrayList<>();
        for (int i = 0; i < n; i++) if (adj.get(i).size() == 1) leaves.add(i);
        
        while (n > 2) {
            n -= leaves.size();
            List<Integer> newLeaves = new ArrayList<>();
            for (int leaf : leaves) {
                int neighbor = adj.get(leaf).iterator().next();
                adj.get(neighbor).remove(leaf);
                if (adj.get(neighbor).size() == 1) newLeaves.add(neighbor);
            }
            leaves = newLeaves;
        }
        
        return leaves;
    }
}
```

c#
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public IList<int> FindMinHeightTrees(int n, int[][] edges) {
        if (n == 1) return new List<int> { 0 };
        
        List<HashSet<int>> adj = new List<HashSet<int>>();
        for (int i = 0; i < n; i++) adj.Add(new HashSet<int>());
        foreach (var edge in edges) {
            adj[edge[0]].Add(edge[1]);
            adj[edge[1]].Add(edge[0]);
        }
        
        List<int> leaves = new List<int>();
        for (int i = 0; i < n; i++) if (adj[i].Count == 1) leaves.Add(i);
        
        while (n > 2) {
            n -= leaves.Count;
            List<int> newLeaves = new List<int>();
            foreach (var leaf in leaves) {
                int neighbor = -1;
                foreach (var nbor in adj[leaf]) {
                    neighbor = nbor;
                    break;
                }
                adj[neighbor].Remove(leaf);
                if (adj[neighbor].Count == 1) newLeaves.Add(neighbor);
            }
            leaves = newLeaves;
        }
        
        return leaves;
    }
}
```

