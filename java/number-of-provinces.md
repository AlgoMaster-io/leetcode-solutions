# [Leetcode 547: Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Navigation
- [Approach 1: Depth-First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Breadth-First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Union-Find](#approach-3-union-find)

----

## Approach 1: Depth-First Search (DFS)

### Intuition
The problem can be conceptualized as finding connected components in an undirected graph. Each city is a node, and a direct road between two cities is an edge. Using Depth-First Search, we can explore all nodes (i.e., cities) connected to a starting node, marking them as visited. Each new unvisited node indicates the start of a new province.

### Code
```java
public class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        boolean[] visited = new boolean[n];
        int numProvinces = 0;
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(isConnected, visited, i);
                numProvinces++;
            }
        }
        
        return numProvinces;
    }
    
    private void dfs(int[][] isConnected, boolean[] visited, int city) {
        visited[city] = true; // Mark the city as visited
        for (int i = 0; i < isConnected.length; i++) {
            // If there is a road from `city` to `i` and `i` hasn't been visited
            if (isConnected[city][i] == 1 && !visited[i]) {
                dfs(isConnected, visited, i); // Explore city `i`
            }
        }
    }
}
```

### Time Complexity
- O(n^2): We visit each cell in the matrix isConnected exactly once.

### Space Complexity
- O(n): The space for the visited array and the call stack in the worst case (when the graph is a single connected component).

----

## Approach 2: Breadth-First Search (BFS)

### Intuition
Similar to the DFS approach, we aim to find connected components. However, BFS uses a queue to explore all nodes extensively level by level. Whenever we find a new city that hasn't been visited, we add it to the queue and mark it as part of the same province.

### Code
```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        boolean[] visited = new boolean[n];
        int numProvinces = 0;
        
        Queue<Integer> queue = new LinkedList<>();
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                queue.add(i);
                while (!queue.isEmpty()) {
                    int city = queue.poll();
                    visited[city] = true;
                    for (int j = 0; j < n; j++) {
                        if (isConnected[city][j] == 1 && !visited[j]) {
                            queue.add(j);
                        }
                    }
                }
                numProvinces++;
            }
        }
        
        return numProvinces;
    }
}
```

### Time Complexity
- O(n^2): Every city and every possible connection (road) is considered.

### Space Complexity
- O(n): The space for the visited array and the queue.

----

## Approach 3: Union-Find

### Intuition
Union-Find efficiently tracks and merges disjoint sets, or connected components, which suits this problem perfectly. Initially, each city is its own province. As we process direct roads between cities, we union their sets. The number of unique roots in the Union-Find structure at the end will give us the number of provinces.

### Code
```java
public class Solution {

    class UnionFind {
        private int[] parent;
        private int count;

        UnionFind(int n) {
            parent = new int[n];
            count = n;
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
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
                parent[rootX] = rootY; // Union by rank could improve this
                count--;
            }
        }

        public int getCount() {
            return count;
        }
    }

    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        UnionFind uf = new UnionFind(n);
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (isConnected[i][j] == 1) {
                    uf.union(i, j);
                }
            }
        }
        
        return uf.getCount();
    }
}
```

### Time Complexity
- O(n^2): Iterate through the upper triangle of the matrix.

### Space Complexity
- O(n): Additional space used by the Union-Find structure.

