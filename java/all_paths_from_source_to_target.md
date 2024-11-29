# 797. [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approach: Depth-First Search (DFS)

### Solution
```java
// Time Complexity: O(2^n * n), where n is the number of nodes in the graph
// Space Complexity: O(2^n * n), for storing all paths in the result
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        path.add(0); // Start from the source node
        dfs(graph, 0, path, result);
        return result;
    }

    private void dfs(int[][] graph, int node, List<Integer> path, List<List<Integer>> result) {
        if (node == graph.length - 1) {
            // If we reach the target node, add the current path to the result
            result.add(new ArrayList<>(path));
            return;
        }

        for (int neighbor : graph[node]) {
            path.add(neighbor); // Add the neighbor to the current path
            dfs(graph, neighbor, path, result); // Recur for the neighbor
            path.remove(path.size() - 1); // Backtrack to explore other paths
        }
    }
}
```