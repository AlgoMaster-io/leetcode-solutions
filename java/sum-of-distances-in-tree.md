### [Leetcode Problem 834: Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

---

#### Approaches
- [Approach 1: Brute Force using DFS](#approach-1)
- [Approach 2: Optimized DFS with DP and Precomputation](#approach-2)

---

### Approach 1: Brute Force using DFS <a name="approach-1"></a>

**Intuition:**

The brute force approach calculates the distance for each node by performing a Depth First Search (DFS) from each node to every other node. This gives a clear idea of the problem but is inefficient due to repeatedly traversing the tree for each node.

**Detailed Steps:**

1. For each node in the tree, calculate the sum of distances to all other nodes by performing a DFS traversal.
2. Maintain a graph representation using adjacency lists for bi-directional traversal.
3. After computing the distance sum for one node, repeat the procedure for all nodes.

This approach is clear but inefficient due to repeated calculations, making it not suitable for larger trees.

```java
import java.util.*;

public class Solution {
    private List<Set<Integer>> graph;
    private int[] answer;
    private boolean[] visited;
    
    public int[] sumOfDistancesInTree(int N, int[][] edges) {
        graph = new ArrayList<>();
        answer = new int[N];
        
        // Initialize graph as adjacency list
        for (int i = 0; i < N; i++) {
            graph.add(new HashSet<>());
        }
        
        // Fill the graph
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }
        
        // Calculate distance sums for each node as the root
        for (int i = 0; i < N; i++) {
            visited = new boolean[N];
            answer[i] = dfs(i, visited);
        }
        
        return answer;
    }
    
    private int dfs(int node, boolean[] visited) {
        visited[node] = true;
        int result = 0;
        
        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                result += dfs(neighbor, visited) + 1;
            }
        }
        
        return result;
    }
}

```

**Time Complexity:** O(N^2) due to DFS from each node.

**Space Complexity:** O(N) for storing graph as adjacency list and visited array.

---

### Approach 2: Optimized DFS with DP and Precomputation <a name="approach-2"></a>

**Intuition:**

To optimize, we use dynamic programming and precompute sub-tree information to reuse calculations efficiently. We perform two DFS traversals, where the first one calculates the sum of distances from an arbitrary root (e.g., node 0) to all its sub-nodes, and the second adjusts these values from the root to other nodes using precomputed information.

**Detailed Steps:**

1. Perform a first DFS to calculate:
   - `count[node]`: number of nodes in the subtree rooted at `node`.
   - `answer[0]`: initial sum of distances from root node to all other nodes.
2. Execute a second DFS to propagate these sums effectively to the rest of the nodes and adjust them using the precomputed values:
   - For each node transitioning from parent, calculate its sum based on the child's distance by subtracting its distance contributed and adding distances from non-subtree nodes.

**Code:**

```java
import java.util.*;

public class Solution {
    private List<Set<Integer>> graph;
    private int[] answer;
    private int[] count;
    
    public int[] sumOfDistancesInTree(int N, int[][] edges) {
        graph = new ArrayList<>();
        answer = new int[N];
        count = new int[N];
        
        // Initialize graph representation
        for (int i = 0; i < N; i++) {
            graph.add(new HashSet<>());
        }
        
        // Fill the graph
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }
        
        // First DFS to calculate initial distance sum and count nodes
        dfs(0, -1);
        // Second DFS to calculate the result for all nodes based on the root
        dfs2(0, -1);
        
        return answer;
    }
    
    private void dfs(int node, int parent) {
        for (int neighbor : graph.get(node)) {
            if (neighbor == parent) continue;
            dfs(neighbor, node);
            count[node] += count[neighbor];
            answer[node] += answer[neighbor] + count[neighbor];
        }
        count[node]++;
    }
    
    private void dfs2(int node, int parent) {
        for (int neighbor : graph.get(node)) {
            if (neighbor == parent) continue;
            answer[neighbor] = answer[node] - count[neighbor] + (count.length - count[neighbor]);
            dfs2(neighbor, node);
        }
    }
}

```

**Time Complexity:** O(N), as each edge and node is traversed a constant number of times.

**Space Complexity:** O(N) for graph storage and arrays to store results and counts.

