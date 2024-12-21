# [Leetcode 547: Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Approaches:
1. [Depth First Search (DFS) Approach](#dfs-approach)
2. [Breadth First Search (BFS) Approach](#bfs-approach)
3. [Union-Find Approach](#union-find-approach)

### DFS Approach

**Intuition:**

The problem can be seen as finding the number of connected components in an undirected graph. Each city (node) is connected directly or indirectly to other cities (nodes). We can use DFS to explore all nodes in one connected component starting from an arbitrary node, and mark them visited. Every time we encounter an unvisited node, it indicates a new province.

1. Create a `visited` array to keep track of visited nodes.
2. Traverse each node, if it is not visited, increment the province count and start a DFS from that node.
3. In DFS, mark the current node as visited and recursively apply DFS for its neighbors that are directly connected and not yet visited.
   
**Code:**

```csharp
public class Solution {
    public int FindCircleNum(int[][] isConnected) {
        int n = isConnected.Length;
        bool[] visited = new bool[n];
        int provinceCount = 0;

        void Dfs(int city) {
            for (int otherCity = 0; otherCity < n; otherCity++) {
                // If city is connected to otherCity and otherCity is not visited
                if (isConnected[city][otherCity] == 1 && !visited[otherCity]) {
                    visited[otherCity] = true; // Mark as visited
                    Dfs(otherCity); // Explore its neighbors
                }
            }
        }

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                provinceCount++;
                Dfs(i); // New province, start DFS
            }
        }

        return provinceCount;
    }
}
```

**Time Complexity:** O(n^2) where n is the number of nodes (cities). Each DFS visit potentially explores n nodes.

**Space Complexity:** O(n) for the `visited` array and recursive call stack in the worst case.

### BFS Approach

**Intuition:**

Similar to DFS, BFS can be used to determine the number of connected components. The advantage of BFS is that it uses a queue data structure and could consume less stack memory than recursive DFS calls in a deep graph.

1. Create a `visited` array to keep track of visited nodes.
2. Traverse each node, if it is not visited, increment the province count and start a BFS from that node.
3. In BFS, add the starting node to a queue. Then, iteratively process each node, marking it visited and enqueuing its unvisited direct connections.

**Code:**

```csharp
public class Solution {
    public int FindCircleNum(int[][] isConnected) {
        int n = isConnected.Length;
        bool[] visited = new bool[n];
        int provinceCount = 0;

        void Bfs(int city) {
            Queue<int> queue = new Queue<int>();
            queue.Enqueue(city);

            while (queue.Count > 0) {
                int currentCity = queue.Dequeue();
                for (int otherCity = 0; otherCity < n; otherCity++) {
                    if (isConnected[currentCity][otherCity] == 1 && !visited[otherCity]) {
                        visited[otherCity] = true; // Mark as visited
                        queue.Enqueue(otherCity); // Add it for further exploration
                    }
                }
            }
        }

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                provinceCount++;
                Bfs(i); // New province, start BFS
            }
        }

        return provinceCount;
    }
}
```

**Time Complexity:** O(n^2) where n is the number of nodes.

**Space Complexity:** O(n) for the `visited` array and queue storage.

### Union-Find Approach

**Intuition:**

The Union-Find (Disjoint Set Union) approach efficiently solves the problem by grouping connected nodes. Each city initially is its own unique province. As we find connections between cities, we perform union operations to group them into the same set. The number of distinct roots at the end gives us the number of provinces.

1. Initialize each city as its parent in an array, where the index represents each city.
2. For each connection, perform a union operation to connect two cities.
3. Use path compression in the find operation to speed up union operations.
4. Count distinct roots in the parent array which indicates separate provinces.

**Code:**

```csharp
public class Solution {
    public int FindCircleNum(int[][] isConnected) {
        int n = isConnected.Length;
        int[] parent = new int[n];

        // Initialize each node to be its own parent
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }

        int Find(int city) {
            if (parent[city] != city) {
                parent[city] = Find(parent[city]); // Path compression
            }
            return parent[city];
        }

        void Union(int city1, int city2) {
            int root1 = Find(city1);
            int root2 = Find(city2);
            if (root1 != root2) {
                parent[root1] = root2; // Unite the sets
            }
        }

        // Union cities if there's a direct connection
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) { // Only need to process upper triangle (i < j)
                if (isConnected[i][j] == 1) {
                    Union(i, j);
                }
            }
        }

        // Count unique roots which represent distinct provinces
        int provinceCount = 0;
        for (int i = 0; i < n; i++) {
            if (Find(i) == i) {
                provinceCount++;
            }
        }

        return provinceCount;
    }
}
```

**Time Complexity:** O(n^2 * α(n)), where α is the inverse Ackermann function, very slow-growing.

**Space Complexity:** O(n) for the parent array.

These solutions demonstrate progressively efficient ways to solve the `Number of Provinces` problem using basic graph traversal methods and more advanced techniques like Union-Find.

