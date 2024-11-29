# 547. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Approach 1: Depth-First Search (DFS)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(n)
public class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        boolean[] visited = new boolean[n];
        int provinces = 0;

        // Perform DFS for each city if not visited
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(isConnected, visited, i);
                provinces++; // Each DFS completes a province
            }
        }

        return provinces;
    }

    private void dfs(int[][] isConnected, boolean[] visited, int i) {
        visited[i] = true; // Mark the city as visited
        for (int j = 0; j < isConnected.length; j++) {
            // Visit connected cities that have not been visited
            if (isConnected[i][j] == 1 && !visited[j]) {
                dfs(isConnected, visited, j);
            }
        }
    }
}
```

## Approach 2: Breadth-First Search (BFS)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(n)
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        boolean[] visited = new boolean[n];
        int provinces = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                bfs(isConnected, visited, i);
                provinces++;
            }
        }

        return provinces;
    }

    private void bfs(int[][] isConnected, boolean[] visited, int i) {
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(i); // Start BFS with city i
        visited[i] = true; // Mark as visited

        while (!queue.isEmpty()) {
            int city = queue.poll();
            for (int j = 0; j < isConnected.length; j++) {
                // Enqueue connected and unvisited cities
                if (isConnected[city][j] == 1 && !visited[j]) {
                    queue.offer(j);
                    visited[j] = true;
                }
            }
        }
    }
}
```

## Approach 3: Union-Find

### Solution
```java
// Time Complexity: O(n^2 * α(n))
// Space Complexity: O(n)
public class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        UnionFind uf = new UnionFind(n);

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (isConnected[i][j] == 1) {
                    uf.union(i, j); // Union the connected cities
                }
            }
        }

        return uf.getCount(); // Return the number of provinces
    }
}

class UnionFind {
    private int[] parent;
    private int count; // Number of provinces

    public UnionFind(int n) {
        parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i; // Initial root is itself
        }
        count = n;
    }

    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }

    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            parent[rootX] = rootY; // Union by root
            count--;
        }
    }

    public int getCount() {
        return count;
    }
}
```

