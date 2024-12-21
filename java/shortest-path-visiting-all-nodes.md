# [Leetcode 847: Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

## Table of Contents
- [Approach 1: BFS Traversal](#approach-1-bfs-traversal)
- [Approach 2: Dynamic Programming with Bitmasking](#approach-2-dynamic-programming-with-bitmasking)

## Approach 1: BFS Traversal

### Intuition
The problem can be translated into finding the shortest path covering all nodes in a graph, known as the Traveling Salesman Problem (TSP). A breadth-first search (BFS) approach is useful here since it explores all nodes layer by layer, ensuring that the shortest path is found efficiently. By keeping track of the visited nodes using a bitmask for each queue state, we can efficiently manage what nodes have been visited so far.

### Algorithm
1. Use a queue to perform BFS where each state contains the node index and the bitmask of visited nodes.
2. Start from each node by pushing all nodes into the queue as starting points.
3. For each state, explore all its neighbors and update the visited nodes' bitmask accordingly.
4. If all nodes are visited (bitmask becomes `111...1`), return the current path length as the result.
5. Use a set to keep track of visited states to prevent revisiting the same state, optimizing duplication handling.

### Code
```java
import java.util.*;

public class Solution {
    public int shortestPathLength(int[][] graph) {
        int n = graph.length;
        int finalState = (1 << n) - 1;
        Queue<int[]> queue = new LinkedList<>();
        boolean[][] visited = new boolean[n][1 << n];
        
        // Initialize the queue with all nodes (starting from each node)
        for (int i = 0; i < n; i++) {
            queue.add(new int[]{i, 1 << i});
        }
        
        int steps = 0;
        
        // Perform BFS
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] current = queue.poll();
                int node = current[0];
                int state = current[1];
                
                // If all nodes are visited
                if (state == finalState) {
                    return steps;
                }
                
                // Check all adjacent nodes
                for (int neighbor : graph[node]) {                    
                    int nextState = state | (1 << neighbor);
                    if (!visited[neighbor][nextState]) {
                        visited[neighbor][nextState] = true;
                        queue.add(new int[]{neighbor, nextState});
                    }
                }
            }
            steps++;
        }
        
        return -1; // Only for compilation; logical flow should never reach here due to graph connectivity
    }
}
```

### Complexity Analysis
- **Time Complexity**: \(O(2^n \times n^2)\) - There are \(2^n\) possible states for each node, and up to \(n^2\) transitions between nodes.
- **Space Complexity**: \(O(2^n \times n)\) - Space for the `visited` state-array, helping to keep track of node states.

## Approach 2: Dynamic Programming with Bitmasking

### Intuition
This approach uses dynamic programming (DP) in combination with bitmasking to systematically compute the minimum path step required to visit all nodes starting from any node. The bitmask represents the set of visited nodes, providing a compact way to represent subsets of nodes.

### Algorithm
1. Define `dp[mask][i]` as the shortest path that visits the set of nodes represented by `mask` ending at node `i`.
2. For each node, initialize `dp[1 << i][i] = 0` since it costs 0 steps to reach node `i` when starting at `i`.
3. Update the `dp` table by considering transitions between nodes, updating the states for visited nodes' masks.
4. The final answer is the minimum value of `dp[(1 << n) - 1][i]` for all `i`, as it represents visiting all nodes (mask `111...1`).

### Code
```java
import java.util.*;

public class Solution {
    public int shortestPathLength(int[][] graph) {
        int n = graph.length;
        int[][] dp = new int[1 << n][n];
        for (int[] row : dp) Arrays.fill(row, Integer.MAX_VALUE);
        
        Queue<int[]> queue = new LinkedList<>();
        
        for (int i = 0; i < n; i++) {
            dp[1 << i][i] = 0;
            queue.add(new int[]{1 << i, i});
        }
        
        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int mask = current[0];
            int u = current[1];
            
            for (int v : graph[u]) {
                int nextMask = mask | (1 << v);
                if (dp[nextMask][v] > dp[mask][u] + 1) {
                    dp[nextMask][v] = dp[mask][u] + 1;
                    queue.add(new int[]{nextMask, v});
                }
            }
        }
        
        int minPath = Integer.MAX_VALUE;
        for (int i = 0; i < n; i++) {
            minPath = Math.min(minPath, dp[(1 << n) - 1][i]);
        }
        return minPath;
    }
}
```

### Complexity Analysis
- **Time Complexity**: \(O(2^n \times n^2)\) - Each state `(mask, i)` is updated through \(n^2\) possibilities.
- **Space Complexity**: \(O(2^n \times n)\) - Space used by the DP table.

By employing such approaches, we efficiently solve the problem using classic graph traversal techniques and optimization heuristics like bitmask DP, aligning with the constraints and providing a robust solution.

