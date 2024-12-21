# [Leetcode 802: Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

## Approaches
- [Approach 1: DFS with Cycle Detection](#approach-1-dfs-with-cycle-detection)
- [Approach 2: Reverse Graph and Topological Sort](#approach-2-reverse-graph-and-topological-sort)

---

## Approach 1: DFS with Cycle Detection

### Intuition
The idea here is to perform a DFS on each node and track if it can lead to a cycle or not. A "cycle" here indicates that the node is not safe as from that node, there exists a path that returns to itself. We will use a state array for each node to keep track of:

- `0` if the node has not been visited.
- `1` if the node is visiting (visited while its path not finished).
- `2` if the node is safe (visited and no cycle through it).

On visiting a node, if we encounter another node that is in the `visiting` state, it means there's a cycle.

### Code

```java
import java.util.ArrayList;
import java.util.List;

public class FindEventualSafeStates {

    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        int[] state = new int[n]; // 0 = unvisited, 1 = visiting, 2 = safe
        List<Integer> safeNodes = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            if (dfs(i, graph, state)) { 
                safeNodes.add(i); // add to result if node is safe
            }
        }

        return safeNodes;
    }

    private boolean dfs(int node, int[][] graph, int[] state) {
        if (state[node] != 0) {
            return state[node] == 2; // if already safely determined, return true or false accordingly
        }

        state[node] = 1; // mark node as visiting

        for (int neighbor : graph[node]) {
            if (state[neighbor] == 2) { 
                continue; // safe node continue
            }
            if (state[neighbor] == 1 || !dfs(neighbor, graph, state)) {
                return false; // cycle found or leads to unsafe node
            }
        }

        state[node] = 2; // mark the node as safe
        return true;
    }
}
```

### Time Complexity
- **O(V + E)**: We visit each node and each edge exactly once.

### Space Complexity
- **O(V)**: The space is used mainly for the state array, call stack in DFS recursion, and the answer list.

---

## Approach 2: Reverse Graph and Topological Sort

### Intuition
The second approach involves inverting the edges of the graph. A node is safe if it eventually leads to a terminal node, which is an outdegree of 0 in the original graph. By reversing the graph's edges, terminal nodes convert to indegree 0 nodes.

Use a queue to perform a Kahn's algorithm variant of topological sort. Begin by enqueueing nodes with outdegree 0 (become indegree 0 in reversed graph) and iteratively remove them, updating neighbors' indegree count.

### Code

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class FindEventualSafeStates {

    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        List<List<Integer>> reverseGraph = new ArrayList<>();
        int[] outdegree = new int[n];
        Queue<Integer> queue = new LinkedList<>();
        List<Integer> safeNodes = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            reverseGraph.add(new ArrayList<>());
        }

        for (int i = 0; i < n; i++) {
            for (int node : graph[i]) {
                reverseGraph.get(node).add(i); // reverse the edge
            }
            outdegree[i] = graph[i].length; // calculate outdegree for each node
            if (outdegree[i] == 0) {
                queue.add(i); // nodes with 0 outdegree (terminal nodes)
            }
        }

        // Perform a reverse topological sort
        while (!queue.isEmpty()) {
            int node = queue.poll();
            safeNodes.add(node); // safe node
            for (int neighbor : reverseGraph.get(node)) {
                outdegree[neighbor]--; // reduce the outdegree of neighbors
                if (outdegree[neighbor] == 0) {
                    queue.add(neighbor); // add new terminal nodes
                }
            }
        }

        Collections.sort(safeNodes); // sort result as expected criteria
        return safeNodes;
    }
}
```

### Time Complexity
- **O(V + E)**: Each node and edge is processed once during the reverse graph creation and queue processing.

### Space Complexity
- **O(V + E)**: Storage for the reverse graph and the queue.

This solution utilizes the properties of graph traversal and reverses the process by making nodes the leaf (or safe nodes) in the topological order of the reversed graph.

