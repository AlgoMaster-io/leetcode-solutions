# 797. [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approach: Depth-First Search (DFS)

### Solution
csharp
```csharp
// Time Complexity: O(2^n * n), where n is the number of nodes in the graph
// Space Complexity: O(2^n * n), for storing all paths in the result
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> AllPathsSourceTarget(int[][] graph) {
        List<IList<int>> result = new List<IList<int>>();
        List<int> path = new List<int>();
        path.Add(0); // Start from the source node
        DFS(graph, 0, path, result);
        return result;
    }

    private void DFS(int[][] graph, int node, List<int> path, List<IList<int>> result) {
        if (node == graph.Length - 1) {
            // If we reach the target node, add the current path to the result
            result.Add(new List<int>(path));
            return;
        }

        foreach (int neighbor in graph[node]) {
            path.Add(neighbor); // Add the neighbor to the current path
            DFS(graph, neighbor, path, result); // Recur for the neighbor
            path.RemoveAt(path.Count - 1); // Backtrack to explore other paths
        }
    }
}
```

