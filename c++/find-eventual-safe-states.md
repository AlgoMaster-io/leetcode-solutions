### [Leetcode 802: Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

#### Approaches:
- [Approach 1: DFS with Coloring Method](#approach-1-dfs-with-coloring-method)
- [Approach 2: Topological Sort with Reverse Graph](#approach-2-topological-sort-with-reverse-graph)

---

### Approach 1: DFS with Coloring Method

**Intuition:**

The problem can be solved efficiently using a Depth First Search (DFS) with a coloring method where:

- Starting from **GRAY** (visiting), 
- Moving to **BLACK** (part of a cycle, not safe),
- Determine as **WHITE** (safe) if it's part of a safe path.

Nodes are eventually safe if they don't lead to a cycle, which effectively means we are looking for nodes that eventually lead to terminal nodes (end of all paths).

**Steps:**

- Use an array `colors` where:
  - **0**: Node is unvisited
  - **1**: Node is visiting (part of current DFS path)
  - **2**: Node is safe
  - **3**: Node is unsafe
  
- Perform DFS on each node:
  1. Mark the node as `1` (visiting).
  2. Recursively visit all connected nodes.
  3. If adjacent node is already `1`, then there's a cycle.
  4. If all adjacent nodes return safe, mark current node as `2` (safe).
  5. If any adjacent node is unsafe, mark current node as `3` (unsafe).

**Code:**

```cpp
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> colors(n, 0); // 0: unvisited, 1: visiting, 2: safe, 3: unsafe
        vector<int> safeNodes;

        function<bool(int)> isSafe = [&](int node) {
            if (colors[node] != 0) {
                return colors[node] == 2;
            }

            // Mark node as visiting
            colors[node] = 1;

            for (int neighbor : graph[node]) { // Traverse all neighbours
                if (!isSafe(neighbor)) { // If any neighbour is not safe
                    colors[node] = 3; // Mark node as unsafe
                    return false;
                }
            }

            colors[node] = 2; // Mark node as safe
            return true;
        };
        
        for (int i = 0; i < n; ++i) {
            if (isSafe(i)) {
                safeNodes.push_back(i);
            }
        }
        
        return safeNodes;
    }
};
```

**Time Complexity:**

- O(V + E), where V is the number of vertices and E is the number of edges, since each node and edge is visited once.

**Space Complexity:**

- O(V), where V is the number of vertices, for the colors array and recursion stack.

---

### Approach 2: Topological Sort with Reverse Graph

**Intuition:**

The problem can also be approached with Topological Sort by reversing the graph. We treat terminal states as starting points and propagate eventual safety backwards.

- Reverse the graph.
- Find nodes with zero out-degree in reversed graph.
- Use a queue to keep track and process nodes with zero out-degree similar to topological sorting.

**Steps:**

- Construct the reversed graph and calculate in-degree for each node.
- Initiate a queue with all terminal nodes (zero in-degree in reversed graph).
- Process nodes from the queue, update in-degree of connected nodes.
- Collect nodes that get processed as safe nodes.

**Code:**

```cpp
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<vector<int>> reverseGraph(n);
        vector<int> outDegree(n, 0);

        for (int u = 0; u < n; ++u) {
            for (int v : graph[u]) {
                reverseGraph[v].push_back(u);
                outDegree[u]++;
            }
        }

        queue<int> q;
        for (int i = 0; i < n; ++i) {
            if (outDegree[i] == 0) { // Terminal nodes
                q.push(i);
            }
        }

        vector<int> safeNodes;
        vector<bool> isSafe(n, false);

        while (!q.empty()) {
            int node = q.front();
            q.pop();
            isSafe[node] = true; // Node is safe

            for (int precursor : reverseGraph[node]) { // Traverse predecessors
                outDegree[precursor]--;
                if (outDegree[precursor] == 0) {
                    q.push(precursor);
                }
            }
        }

        for (int i = 0; i < n; ++i) {
            if (isSafe[i]) {
                safeNodes.push_back(i);
            }
        }

        sort(safeNodes.begin(), safeNodes.end());
        return safeNodes;
    }
}
```

**Time Complexity:**

- O(V + E), where V is the number of vertices and E is the number of edges.

**Space Complexity:**

- O(V + E), for storing the reversed graph and queue.

This approach simplifies determining the safe nodes efficiently by employing a reversed graph concept to backtrack eventual safe states.

