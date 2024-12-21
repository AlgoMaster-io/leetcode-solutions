# [Leetcode 797: All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approaches

1. [Depth First Search (DFS) Recursive Approach](#depth-first-search-dfs-recursive-approach)
2. [DFS with Backtracking](#dfs-with-backtracking)

---

### Depth First Search (DFS) Recursive Approach

The simplest way to solve this problem is to use a Depth First Search (DFS) approach. In this approach, we can recursively explore all paths from the source node to the target node by traversing on each node's neighbors. This uses recursion to explore every possible path and store the ones that reach the target node.

**Intuition:**

- Start from the source node and recursively explore each of its neighbors.
- Append the current node to the path and when node is the last node (target), add the path to results.
- This continues until all possible paths are covered.

**Time Complexity:**
- \(O(2^V \times V)\), where \(V\) is the number of vertices. In the worst case, every node is connected to every other node, leading to \(2^V\) paths.

**Space Complexity:**
- \(O(V)\), consuming space for the call stack.

```csharp
public class Solution {
    public IList<IList<int>> AllPathsSourceTarget(int[][] graph) {
        // List to store all paths from source to target
        List<IList<int>> paths = new List<IList<int>>();
        // Starting DFS search from the first node (0)
        DFS(graph, 0, new List<int>(), paths);
        return paths;
    }

    private void DFS(int[][] graph, int node, List<int> path, List<IList<int>> paths) {
        // Add current node to the path
        path.Add(node);
        
        // If the current node is the last node, add the path to the result list
        if (node == graph.Length - 1) {
            paths.Add(new List<int>(path));
        } else {
            // Recurse through all neighboring nodes
            foreach (var neighbor in graph[node]) {
                DFS(graph, neighbor, path, paths);
            }
        }
        
        // Remove the current node from path to backtrack
        path.RemoveAt(path.Count - 1);
    }
}
```

---

### DFS with Backtracking

We can enhance our basic DFS approach using a backtracking technique. In this method, after exploring a path completely, we remove the last node from our path to explore the next possibility.

**Intuition:**

- Similar to basic DFS, but we explicitly backtrack by removing nodes from our path once we have explored all possible paths from that node.
- This ensures we utilize the path variable efficiently without using extra lists for copying paths at every step.

**Time Complexity:**
- \(O(2^V \times V)\), similar reasoning as above.

**Space Complexity:**
- \(O(V)\), for the path to store nodes and call stack.

The following is an identical implementation with an emphasis on the backtracking process:

```csharp
public class Solution {
    public IList<IList<int>> AllPathsSourceTarget(int[][] graph) {
        List<IList<int>> result = new List<IList<int>>();
        List<int> path = new List<int>();
        DFS(graph, 0, path, result);
        return result;
    }

    private void DFS(int[][] graph, int node, List<int> path, List<IList<int>> result) {
        // Add the current node to our path
        path.Add(node);

        // If the target node is reached
        if (node == graph.Length - 1) {
            result.Add(new List<int>(path)); // Add copy of the path to results
        } else {
            // Visit the next node
            foreach (var next in graph[node]) {
                DFS(graph, next, path, result);
            }
        }

        // Backtrack: Remove current node from path before returning to caller
        path.RemoveAt(path.Count - 1);
    }
}
```

Both of these approaches leverage DFS to explore possible paths from the source to the target, with the addition of backtracking ensuring paths are correctly managed.

