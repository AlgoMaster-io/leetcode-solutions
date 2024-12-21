# [LeetCode 310: Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

## Solutions:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Prune Leaves (Improved)](#approach-2-prune-leaves)

---

### Approach 1: Brute Force

**Intuition**:  
In the brute force approach, we attempt to consider each node as a potential root of the Minimum Height Tree (MHT) and calculate the height for each configuration. The nodes that yield the smallest height will be our result. This approach will give us correct results but is not efficient due to repeated computation of tree heights.

**Steps**:
1. For each node, consider it as the root and use BFS/DFS to calculate the height of the tree.
2. Track the minimum height found and the nodes that produce it.

**Code**:

```csharp
public class Solution {
    public IList<int> FindMinHeightTrees(int n, int[][] edges) {
        if (n == 1) return new List<int> { 0 };

        var adjacencyList = new List<int>[n];
        for (int i = 0; i < n; i++) {
            adjacencyList[i] = new List<int>();
        }

        foreach (var edge in edges) {
            adjacencyList[edge[0]].Add(edge[1]);
            adjacencyList[edge[1]].Add(edge[0]);
        }

        int minHeight = int.MaxValue;
        var result = new List<int>();

        for (int i = 0; i < n; i++) {
            var currentHeight = GetTreeHeight(i, adjacencyList, n);
            if (currentHeight < minHeight) {
                minHeight = currentHeight;
                result.Clear();
                result.Add(i);
            } else if (currentHeight == minHeight) {
                result.Add(i);
            }
        }

        return result;
    }

    private int GetTreeHeight(int root, List<int>[] adjacencyList, int n) {
        var visited = new bool[n];
        var queue = new Queue<int>();
        queue.Enqueue(root);
        visited[root] = true;
        int height = -1;

        while (queue.Count > 0) {
            int count = queue.Count;
            height++;
            for (int i = 0; i < count; i++) {
                int current = queue.Dequeue();
                foreach (var neighbor in adjacencyList[current]) {
                    if (!visited[neighbor]) {
                        visited[neighbor] = true;
                        queue.Enqueue(neighbor);
                    }
                }
            }
        }

        return height;
    }
}
```

**Complexity Analysis**:
- **Time Complexity**: O(n^2) - Performing a BFS/DFS for each node.
- **Space Complexity**: O(n) - Storing adjacency list and visited nodes.

---

### Approach 2: Prune Leaves (Improved)

**Intuition**:  
This approach leverages the properties of trees and graphs. The idea is to repeatedly trim the leaves, the nodes with only one connection, until we reach the core of the graph, which will give us the roots of the Minimum Height Trees. Intuitively, the remaining nodes are the centroids of the tree.

**Steps**:
1. Calculate the initial degree (number of connections) of each node and identify all leaves.
2. Iteratively remove the leaves, updating the degree of neighboring nodes and identifying new leaves.
3. Stop when there are only two or fewer nodes left, as these will be the centroids (roots of MHT).

**Code**:

```csharp
public class Solution {
    public IList<int> FindMinHeightTrees(int n, int[][] edges) {
        if (n == 1) return new List<int> { 0 };

        var adjacencyList = new List<int>[n];
        for (int i = 0; i < n; i++) {
            adjacencyList[i] = new List<int>();
        }

        var degree = new int[n];

        foreach (var edge in edges) {
            adjacencyList[edge[0]].Add(edge[1]);
            adjacencyList[edge[1]].Add(edge[0]);
            degree[edge[0]]++;
            degree[edge[1]]++;
        }

        var leaves = new List<int>();
        for (int i = 0; i < n; i++) {
            if (degree[i] == 1) {
                leaves.Add(i);
            }
        }

        while (n > 2) {
            n -= leaves.Count;
            var newLeaves = new List<int>();

            foreach (int leaf in leaves) {
                // Each leaf has only one neighbor
                foreach (int neighbor in adjacencyList[leaf]) {
                    degree[neighbor]--;
                    if (degree[neighbor] == 1) {
                        newLeaves.Add(neighbor);
                    }
                }
            }

            leaves = newLeaves;
        }

        return leaves;
    }
}
```

**Complexity Analysis**:
- **Time Complexity**: O(n) - Each node and edge is processed once.
- **Space Complexity**: O(n) - Storing adjacency list and degree count.

This approach is significantly more efficient because instead of repeatedly calculating heights, we leverage the properties of trees and reduce the problem using mathematical insights into the structure of graphs.

