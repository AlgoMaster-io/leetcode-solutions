# 547. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Approach 1: Depth-First Search (DFS)

### Solution
c#
// Time Complexity: O(n^2)
// Space Complexity: O(n)
public class Solution {
    public int FindCircleNum(int[][] isConnected) {
        int n = isConnected.Length;
        bool[] visited = new bool[n];
        int provinces = 0;

        // Perform DFS for each city if not visited
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                Dfs(isConnected, visited, i);
                provinces++; // Each DFS completes a province
            }
        }

        return provinces;
    }

    private void Dfs(int[][] isConnected, bool[] visited, int i) {
        visited[i] = true; // Mark the city as visited
        for (int j = 0; j < isConnected.Length; j++) {
            // Visit connected cities that have not been visited
            if (isConnected[i][j] == 1 && !visited[j]) {
                Dfs(isConnected, visited, j);
            }
        }
    }
}


## Approach 2: Breadth-First Search (BFS)

### Solution
c#
// Time Complexity: O(n^2)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int FindCircleNum(int[][] isConnected) {
        int n = isConnected.Length;
        bool[] visited = new bool[n];
        int provinces = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                Bfs(isConnected, visited, i);
                provinces++;
            }
        }

        return provinces;
    }

    private void Bfs(int[][] isConnected, bool[] visited, int i) {
        Queue<int> queue = new Queue<int>();
        queue.Enqueue(i); // Start BFS with city i
        visited[i] = true; // Mark as visited

        while (queue.Count > 0) {
            int city = queue.Dequeue();
            for (int j = 0; j < isConnected.Length; j++) {
                // Enqueue connected and unvisited cities
                if (isConnected[city][j] == 1 && !visited[j]) {
                    queue.Enqueue(j);
                    visited[j] = true;
                }
            }
        }
    }
}


## Approach 3: Union-Find

### Solution
c#
// Time Complexity: O(n^2 * α(n))
// Space Complexity: O(n)
public class Solution {
    public int FindCircleNum(int[][] isConnected) {
        int n = isConnected.Length;
        UnionFind uf = new UnionFind(n);

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (isConnected[i][j] == 1) {
                    uf.Union(i, j); // Union the connected cities
                }
            }
        }

        return uf.GetCount(); // Return the number of provinces
    }
}

public class UnionFind {
    private int[] parent;
    private int count; // Number of provinces

    public UnionFind(int n) {
        parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i; // Initial root is itself
        }
        count = n;
    }

    public int Find(int x) {
        if (parent[x] != x) {
            parent[x] = Find(parent[x]); // Path compression
        }
        return parent[x];
    }

    public void Union(int x, int y) {
        int rootX = Find(x);
        int rootY = Find(y);
        if (rootX != rootY) {
            parent[rootX] = rootY; // Union by root
            count--;
        }
    }

    public int GetCount() {
        return count;
    }
}

