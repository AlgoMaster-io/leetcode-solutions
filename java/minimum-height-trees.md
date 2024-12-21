# [Leetcode 310: Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

## Approaches

- [Approach 1: BFS with Degree Count](#approach-1-bfs-with-degree-count)

---

## Approach 1: BFS with Degree Count

### Intuition

The problem of finding the minimum height trees (MHTs) in an undirected graph can be tackled by understanding that the roots of MHTs are the central nodes in the longest path of a tree. For trees, the diameter path has a central point/points which split the path into equal or nearly equal parts, minimizing height from these nodes.

The key is to iteratively remove leaf nodes (nodes with only one connection) layer by layer until only one or two nodes are left. These final nodes represent the root of the minimum height trees.

### Explanation

1. **Graph Representation:**  
   We first create an adjacency list to represent the graph as this will help us easily find neighbors of each node.

2. **Initial Setup:**  
   - Identify nodes with only one connection (leaves).
   - Use a queue to perform a Breadth-First Search (BFS) starting from these leaves.

3. **Iterative Leaf Removal:**
   - Repeatedly remove leaves level by level until one or two nodes remain. 
   - For each leaf removed, reduce the degree of its neighbor and if the degree becomes one, it becomes a leaf for the next level.

4. **Identify MHT Roots:**
   - When we can't remove any more edges (when the remaining pending nodes are 1 or 2), these nodes are the desired roots of MHTs.

### Code

```java
import java.util.*;

public class MinimumHeightTrees {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if (n == 1) return Collections.singletonList(0);
        
        // Step 1: Build the graph using adjacency list
        List<Set<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new HashSet<>());
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }
        
        // Step 2: Initialize the first layer of leaves
        List<Integer> leaves = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (graph.get(i).size() == 1) leaves.add(i);
        }
        
        // Step 3: Trim the leaves until reaching the centroids
        int remainingNodes = n;
        while (remainingNodes > 2) {
            remainingNodes -= leaves.size();
            List<Integer> newLeaves = new ArrayList<>();
            for (int leaf : leaves) {
                int neighbor = graph.get(leaf).iterator().next();
                graph.get(neighbor).remove(leaf);
                if (graph.get(neighbor).size() == 1) newLeaves.add(neighbor);
            }
            leaves = newLeaves;
        }
        
        return leaves;
    }
}
```

### Time Complexity

- **Time Complexity:** \(O(n)\), where \(n\) is the number of nodes. Each node and edge is added and removed at most once from the list.
  
- **Space Complexity:** \(O(n)\). The space mainly depends on the graph structure and any auxiliary space used for tracking nodes and leaves.

Using this approach, we efficiently compute the minimum height trees by targeting the central propensity of trees and manipulating node connections accordingly.

