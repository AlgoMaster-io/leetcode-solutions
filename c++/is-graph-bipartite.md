[Leetcode 785: Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)

## Approaches
- [Approach 1: DFS with Coloring](#approach-1-dfs-with-coloring)
- [Approach 2: BFS with Coloring](#approach-2-bfs-with-coloring)

### Approach 1: DFS with Coloring

#### Intuition:
To determine if a graph is bipartite, we can utilize a coloring method. A bipartite graph can be colored using two colors such that no two adjacent nodes have the same color. By using a Depth First Search (DFS) approach, we recursively attempt to color the graph. If we encounter a node with the same color as its parent in the DFS traversal, the graph is not bipartite.

#### Steps:
1. Initialize a `color` array to store the color of each node. Use `-1` to indicate uncolored nodes.
2. For each unvisited node in the graph, apply DFS:
   - Attempt to color the node with the opposite color of its parent.
   - If a conflict is detected (a neighboring node has the same color), return `false`.
3. If the whole graph is colored without conflict, return `true`.

#### Code:
```cpp
class Solution {
public:
    bool dfs(vector<vector<int>>& graph, vector<int>& color, int node, int currentColor) {
        color[node] = currentColor;
        
        // Traverse all adjacent nodes
        for (int adjacent : graph[node]) {
            // If the adjacent node is not colored, color it with the opposite color
            if (color[adjacent] == -1) {
                if (!dfs(graph, color, adjacent, 1 - currentColor)) {
                    return false;
                }
            }
            // If the adjacent node is colored with the same color as the current node
            else if (color[adjacent] == color[node]) {
                return false;  // The graph is not bipartite
            }
        }
        return true;
    }
    
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> color(n, -1);  // -1 indicates that the node is uncolored
        
        // Try to color each component
        for (int i = 0; i < n; ++i) {
            if (color[i] == -1) {  // Unvisited node
                if (!dfs(graph, color, i, 0)) {
                    return false;  // If not bipartite, return false
                }
            }
        }
        return true;  // If no conflicts, the graph is bipartite
    }
};
```

#### Time Complexity:
- O(V + E), where V is the number of vertices and E is the number of edges, due to DFS traversal.

#### Space Complexity:
- O(V), required for the recursion stack and color array.

### Approach 2: BFS with Coloring

#### Intuition:
Alternatively, we can also use Breadth First Search (BFS) for coloring the graph in a similar manner. This approach avoids recursion and uses a queue for iterative processing of nodes. Like the DFS method, this approach aims to color each node in an alternating pattern and check for conflicts with neighboring nodes.

#### Steps:
1. Initialize a `color` array just like in DFS.
2. Use a queue to process nodes iteratively, assigning colors.
3. For each uncolored node, start a BFS:
   - Assign colors alternately to each level/nodes.
   - If a node conflicts in color with an adjacent node, return `false`.
4. Continue until all nodes are processed. If no conflicts are found, return `true`.

#### Code:
```cpp
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> color(n, -1);  // -1 indicates that the node is uncolored
        
        for (int i = 0; i < n; ++i) {
            if (color[i] == -1) {  // Unvisited node
                queue<int> q;
                q.push(i);
                color[i] = 0;  // Start coloring the node with color 0
                
                while (!q.empty()) {
                    int node = q.front();
                    q.pop();

                    for (int adjacent : graph[node]) {
                        if (color[adjacent] == -1) {
                            // If the adjacent node is not colored, color it with the opposite color
                            color[adjacent] = 1 - color[node];
                            q.push(adjacent);
                        } else if (color[adjacent] == color[node]) {
                            return false;  // Conflict in coloring, thus not bipartite
                        }
                    }
                }
            }
        }
        return true;  // All nodes successfully colored without conflict
    }
};
```

#### Time Complexity:
- O(V + E), similar to the DFS approach.

#### Space Complexity:
- O(V), for the color array and BFS queue.

These approaches ensure that we systematically check all nodes and edges for any possible bipartiteness by using either depth-wise or breadth-wise graph traversal techniques.

