# [Leetcode 785: Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)

Solutions:
- [Approach 1: Depth First Search (DFS) with Two Colors](#approach-1-depth-first-search-dfs-with-two-colors)
- [Approach 2: Breadth First Search (BFS) with Two Colors](#approach-2-breadth-first-search-bfs-with-two-colors)
- [Conclusion](#conclusion)

## Approach 1: Depth First Search (DFS) with Two Colors

### Intuition

A graph is bipartite if its nodes can be divided into two independent sets such that no two graph nodes within the same set are adjacent. We can utilize a two-color technique, where we attempt to color the graph using two colors and verify that no two adjacent nodes have the same color. This can be efficiently done using Depth First Search (DFS).

### Steps
1. Initialize a color array to keep track of the color assigned to each node (e.g., use -1, 0, and 1, where -1 indicates uncolored).
2. Traverse each node in the graph. If a node is uncolored, apply DFS to color it and its connected components.
3. During DFS, ensure no two adjacent nodes have the same color. If two adjacent nodes share a color, the graph is not bipartite.
4. If all nodes can be properly colored, then the graph is bipartite.

### Java Code

```java
public class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] colors = new int[n];
        // Initialize all vertices as uncolored
        Arrays.fill(colors, -1);
        
        for (int i = 0; i < n; i++) {
            // If the current node is not colored, apply DFS coloring
            if (colors[i] == -1) {
                if (!dfs(graph, colors, i, 0)) {
                    return false;
                }
            }
        }
        return true;
    }
    
    private boolean dfs(int[][] graph, int[] colors, int node, int color) {
        // Assign color to the current node
        colors[node] = color;

        // Traverse all connected nodes
        for (int neighbor : graph[node]) {
            // If the neighbor is not colored, assign the opposite color and continue DFS
            if (colors[neighbor] == -1) {
                if (!dfs(graph, colors, neighbor, 1 - color)) {
                    return false;
                }
            } 
            // If the neighbor has the same color, the graph is not bipartite
            else if (colors[neighbor] == color) {
                return false;
            }
        }
        return true;
    }
}
```

### Complexity
- **Time Complexity**: O(V + E) where V is the number of vertices and E is the number of edges.
- **Space Complexity**: O(V) for storing the color of each node.

## Approach 2: Breadth First Search (BFS) with Two Colors

### Intuition

Another way to solve the problem using two-coloring is to utilize Breadth First Search (BFS). This approach uses a queue to explore each level of the graph and ensures that adjacent nodes are assigned different colors.

### Steps
1. Create a color array similar to the DFS approach.
2. Use a queue to perform BFS on each uncolored component of the graph.
3. Attempt to color each node and its neighbors with alternating colors.
4. If any adjacent nodes have the same color during BFS traversal, the graph is not bipartite.

### Java Code

```java
public class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] colors = new int[n];
        // Initialize all vertices as uncolored
        Arrays.fill(colors, -1);

        for (int i = 0; i < n; i++) {
            if (colors[i] == -1) {
                Queue<Integer> queue = new LinkedList<>();
                queue.offer(i);
                colors[i] = 0; // Start coloring with 0

                while (!queue.isEmpty()) {
                    int current = queue.poll();

                    for (int neighbor : graph[current]) {
                        if (colors[neighbor] == -1) {
                            colors[neighbor] = 1 - colors[current]; // Assign opposite color
                            queue.offer(neighbor);
                        } else if (colors[neighbor] == colors[current]) {
                            return false;
                        }
                    }
                }
            }
        }
        return true;
    }
}
```

### Complexity
- **Time Complexity**: O(V + E) where V is the number of vertices and E is the number of edges (same as DFS).
- **Space Complexity**: O(V) for storing the color of each node and the queue.

## Conclusion

Both DFS and BFS approaches are effective in determining whether a graph is bipartite, and each has the same time and space complexities. The choice between DFS and BFS can depend on the specific constraints and needs of the problem or developer preference.

