# [Leetcode 834: Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized DFS with Precomputing Subtree Sizes and Distances](#approach-2-optimized-dfs-with-precomputing-subtree-sizes-and-distances)

### Approach 1: Brute Force

**Intuition**:  
In this approach, we directly calculate the total distance from each node to all other nodes using BFS or DFS for each node. We calculate the distance from each node to every other node and sum them up.

**Solution**:
- Convert the list of edges into an adjacency list for the graph representation.
- Perform a BFS/DFS starting from each node to compute distances to all others.
- For each node, sum all distances calculated in the previous step and store them.

**Code**:
```csharp
public int[] SumOfDistancesInTree(int N, int[][] edges) {
    // Create the graph as an adjacency list
    var graph = new List<int>[N];
    for (int i = 0; i < N; i++) {
        graph[i] = new List<int>();
    }
    foreach (var edge in edges) {
        graph[edge[0]].Add(edge[1]);
        graph[edge[1]].Add(edge[0]);
    }

    var result = new int[N];

    // BFS/DFS for each node to find sum of distances to all other nodes
    for (int i = 0; i < N; i++) {
        var visited = new bool[N];
        var queue = new Queue<int>();
        var distances = new int[N];
        queue.Enqueue(i);
        visited[i] = true;

        while (queue.Count > 0) {
            var currentNode = queue.Dequeue();
            foreach (int neighbor in graph[currentNode]) {
                if (!visited[neighbor]) {
                    distances[neighbor] = distances[currentNode] + 1;
                    result[i] += distances[neighbor];
                    visited[neighbor] = true;
                    queue.Enqueue(neighbor);
                }
            }
        }
    }

    return result;
}
```

**Complexity Analysis**:
- **Time Complexity**: O(N^2), because for each node we perform a BFS/DFS to every other node.
- **Space Complexity**: O(N), for storing the adjacency list and BFS/DFS structures.

### Approach 2: Optimized DFS with Precomputing Subtree Sizes and Distances

**Intuition**:  
To optimize, we calculate a 'base case' sum of distances using DFS for one node (often node 0). Then, we use this information to efficiently compute sums for other nodes without recalculating distances between all pairs again. The key observation is that moving the root from `u` to one of its children, the sum of distances changes in a predictable way using subtree sizes.

**Solution**:
- First DFS to calculate total distances from an arbitrary root (node 0) and size of subtrees.
- Second DFS to calculate the distances for other nodes using the results from the first DFS.

**Code**:
```csharp
public int[] SumOfDistancesInTree(int N, int[][] edges) {
    var graph = new List<int>[N];
    for (int i = 0; i < N; i++) {
        graph[i] = new List<int>();
    }
    foreach (var edge in edges) {
        graph[edge[0]].Add(edge[1]);
        graph[edge[1]].Add(edge[0]);
    }

    var count = new int[N];
    var result = new int[N];

    void Dfs1(int node, int parent) {
        // Start DFS with the initial subtree size which is the node itself
        count[node] = 1;

        foreach (var neighbor in graph[node]) {
            if (neighbor == parent) continue;
            Dfs1(neighbor, node);
            // Accumulate the subtree size
            count[node] += count[neighbor];
            // Accumulate the distance sum result
            result[node] += result[neighbor] + count[neighbor];
        }
    }

    void Dfs2(int node, int parent) {
        foreach (var neighbor in graph[node]) {
            if (neighbor == parent) continue;
            // Adjust distance sum for the child node
            result[neighbor] = result[node] - count[neighbor] + (N - count[neighbor]);
            Dfs2(neighbor, node);
        }
    }

    // First pass to compute initial distance sums and subtree sizes
    Dfs1(0, -1);
    // Second pass to adjust the total distance sums based on relative positions
    Dfs2(0, -1);

    return result;
}
```

**Complexity Analysis**:
- **Time Complexity**: O(N), because each node and edge is visited a constant number of times during the DFS.
- **Space Complexity**: O(N), for storing graph, count, and result arrays.

