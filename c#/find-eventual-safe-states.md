# [Leetcode 802: Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

### Approaches

- [Approach 1: Graph Traversal with DFS (Basic)](#approach-1-graph-traversal-with-dfs-basic)
- [Approach 2: Topological Sort with Reverse Edges (Optimal)](#approach-2-topological-sort-with-reverse-edges-optimal)

---

## Approach 1: Graph Traversal with DFS (Basic)

### Intuition:

The problem involves finding nodes (states) in a directed graph that do not lead to any cycles eventually. One basic approach is to perform a depth-first search (DFS) on each node and detect whether it leads to a cycle or eventually ends in a terminal node.

### Detailed Steps:

1. Use a `colors` array to track the state of each node:
   - **White (0):** Node is unvisited.
   - **Gray (1):** Node is currently being visited (this is used to detect cycles).
   - **Black (2):** Node is safe, i.e., it has been fully processed and is not part of a cycle.

2. Perform a DFS for each unvisited node. During the traversal:
   - If a node is marked as Gray and it is encountered again, it means a cycle is present.
   - If we complete traversal marking a node as Black, it is safe.

3. Collect all safe nodes (marked as Black) and return them in a sorted order.

### Implementation:

```csharp
public class Solution {
    public IList<int> EventualSafeNodes(int[][] graph) {
        int[] colors = new int[graph.Length];

        // Helper function for DFS
        bool Dfs(int node) {
            if (colors[node] > 0) return colors[node] == 2;
            
            colors[node] = 1; // mark as gray (currently processing)
            foreach (var neighbor in graph[node]) {
                if (colors[neighbor] == 2) continue;
                if (colors[neighbor] == 1 || !Dfs(neighbor)) return false;
            }
            
            colors[node] = 2; // mark as black (safe)
            return true;
        }

        List<int> result = new List<int>();
        for (int i = 0; i < graph.Length; i++) {
            if (Dfs(i)) result.Add(i);
        }
        
        return result;
    }
}
```

### Time Complexity: O(V + E)

- V = number of vertices (nodes), E = number of edges.
- Each node and edge are traversed only a constant number of times.

### Space Complexity: O(V)

- Space for the `colors` array and recursion stack.

---

## Approach 2: Topological Sort with Reverse Edges (Optimal)

### Intuition:

An optimal solution involves recognizing that terminal nodes are those with out-degree zero. By reversing the graph's edges, we can apply a topological sort approach to list all nodes with no incoming edges.

### Detailed Steps:

1. Reverse the edges of the graph. This allows us to track nodes leading to terminal nodes.
2. Compute the in-degree of each node (incoming edges count).
3. Use a queue to perform topological sorting starting from nodes with no incoming edges.
4. Continually dequeue nodes and reduce the in-degree of their neighbors. Nodes that reach an in-degree of 0 are safe.

### Implementation:

```csharp
public class Solution {
    public IList<int> EventualSafeNodes(int[][] graph) {
        int n = graph.Length;
        // Create the reverse graph
        List<int>[] reverseGraph = new List<int>[n];
        for (int i = 0; i < n; i++) {
            reverseGraph[i] = new List<int>();
        }

        int[] indegree = new int[n];

        // Build reverse graph and calculate in-degrees
        for (int i = 0; i < n; i++) {
            foreach (var neighbor in graph[i]) {
                reverseGraph[neighbor].Add(i);
                indegree[i]++;
            }
        }

        // Queue for nodes with zero in-degree
        Queue<int> queue = new Queue<int>();

        // Populate queue with initial terminal nodes
        for (int i = 0; i < n; i++) {
            if (indegree[i] == 0) queue.Enqueue(i);
        }

        List<int> safeNodes = new List<int>();

        // Perform a BFS based on in-degrees
        while (queue.Count > 0) {
            int node = queue.Dequeue();
            safeNodes.Add(node);
            foreach (var neighbor in reverseGraph[node]) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) queue.Enqueue(neighbor);
            }
        }

        safeNodes.Sort();
        return safeNodes;
    }
}
```

### Time Complexity: O(V + E)

- Like DFS, each node and edge are processed a limited number of times.

### Space Complexity: O(V + E)

- Additional space for storing the reversed graph and in-degrees array.

Both solutions identify the eventual safe nodes accurately, with the latter approach offering an edge due to its clearer path towards manifestly terminating nodes through reverse graph traversal.

