# [Leetcode Problem 547: Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Approaches

- [Approach 1: Depth First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Breadth First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Union-Find (Disjoint Set Union)](#approach-3-union-find-disjoint-set-union)

---

## Approach 1: Depth First Search (DFS)

The idea is to use DFS to explore all cities belonging to a single province. By iterating through each city and performing a DFS for unvisited cities, we can count the number of connected components in the graph, which corresponds to the number of provinces.

### Intuition:

1. Traverse each city in the "isConnected" matrix.
2. If a city hasn't been visited, that means we've encountered a new province. Initiate a DFS to mark all connected cities as visited.
3. The DFS function will recursively visit all connected cities (i.e., all cities in the same province).
4. Each initiation of a DFS implies discovering a new province, so increment the province count.

### Time Complexity:
- **O(n^2)**: We potentially have to visit all n cities and check their connection with each other.

### Space Complexity:
- **O(n)**: Space used by the visited array to keep track of visited cities.

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    void dfs(int node, vector<vector<int>>& isConnected, vector<bool>& visited) {
        visited[node] = true; // Mark the current city as visited
        for (int j = 0; j < isConnected.size(); ++j) {
            // If there is a direct connection and the city hasn't been visited, DFS the city
            if (isConnected[node][j] == 1 && !visited[j]) {
                dfs(j, isConnected, visited);
            }
        }
    }
    
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        vector<bool> visited(n, false);
        int provinces = 0;
        
        for (int i = 0; i < n; ++i) {
            // If a city hasn't been visited, start a DFS
            if (!visited[i]) {
                dfs(i, isConnected, visited);
                ++provinces; // Increment the number of provinces
            }
        }
        return provinces;
    }
};
```

---

## Approach 2: Breadth First Search (BFS)

In the BFS approach, instead of using recursion, we use a queue to iteratively explore all cities connected to a given city.

### Intuition:

1. Use a queue to perform BFS starting from each unvisited city.
2. Similar to DFS, each unvisited city denotes a start to a new province.
3. As each city is dequeued and visited, mark it as visited.
4. For each connected city, enqueue it if it hasn’t been visited.

### Time Complexity:
- **O(n^2)**: Same rationale as DFS, as all potential connections are explored.

### Space Complexity:
- **O(n)**: Similar to DFS, but the additional space used by the BFS queue is also O(n).

```cpp
#include <vector>
#include <queue>
using namespace std;

class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        vector<bool> visited(n, false);
        int provinces = 0;
        queue<int> q;
        
        for (int i = 0; i < n; ++i) {
            if (!visited[i]) {
                provinces++; // Start of a new province
                q.push(i);
                
                while (!q.empty()) {
                    int current = q.front();
                    q.pop();
                    visited[current] = true;
                    
                    for (int j = 0; j < n; ++j) {
                        // If connected and not visited, enqueue the city
                        if (isConnected[current][j] == 1 && !visited[j]) {
                            q.push(j);
                        }
                    }
                }
            }
        }
        return provinces;
    }
};
```

---

## Approach 3: Union-Find (Disjoint Set Union)

Union-Find is an optimal approach for finding the number of connected components in a graph. By iteratively joining connected cities and counting distinct root representatives, we determine the number of provinces.

### Intuition:

1. Represent each city initially as its own set.
2. For every pair of cities directly connected, perform a union operation to combine their sets.
3. After processing all connections, the number of unique sets represents the number of provinces.

### Time Complexity:
- **O(n^2 * α(n))**: Typically considered nearly linear, where α(n) is the inverse Ackermann function, very slow-growing and considered constant for practical n. This complexity arises from performing union and find operations efficiently.

### Space Complexity:
- **O(n)**: Space for storing the parent and rank arrays.

```cpp
#include <vector>
using namespace std;

class UnionFind {
    vector<int> root;
    vector<int> rank;
public:
    UnionFind(int size) : root(size), rank(size, 1) {
        for (int i = 0; i < size; ++i) {
            root[i] = i;
        }
    }
    
    int find(int x) {
        if (x == root[x]) {
            return x;
        }
        root[x] = find(root[x]); // Path compression
        return root[x];
    }
    
    bool unionSet(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX != rootY) {
            // Union by rank
            if (rank[rootX] > rank[rootY]) {
                root[rootY] = rootX;
            }
            else if (rank[rootX] < rank[rootY]) {
                root[rootX] = rootY;
            }
            else {
                root[rootY] = rootX;
                rank[rootX] += 1;
            }
            return true;
        }
        return false;
    }
    
    int countDistinctRoots() {
        int count = 0;
        for (int i = 0; i < root.size(); ++i) {
            if (root[i] == i) {
                count++;
            }
        }
        return count;
    }
};

class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        UnionFind uf(n);
        
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (isConnected[i][j] == 1) {
                    uf.unionSet(i, j);
                }
            }
        }
        
        return uf.countDistinctRoots();
    }
};
```

Each approach has its merits, but Union-Find generally offers a balance of efficiency and clarity for connected components in undirected graphs.

