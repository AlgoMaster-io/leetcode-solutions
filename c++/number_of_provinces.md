# 547. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Approach 1: Depth-First Search (DFS)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <vector>

class Solution {
public:
    int findCircleNum(std::vector<std::vector<int>>& isConnected) {
        int n = isConnected.size();
        std::vector<bool> visited(n, false);
        int provinces = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(isConnected, visited, i);
                provinces++;  // Each DFS completes a province
            }
        }

        return provinces;
    }

private:
    void dfs(std::vector<std::vector<int>>& isConnected, std::vector<bool>& visited, int i) {
        visited[i] = true;  // Mark the city as visited
        for (int j = 0; j < isConnected.size(); j++) {
            // Visit connected cities that have not been visited
            if (isConnected[i][j] == 1 && !visited[j]) {
                dfs(isConnected, visited, j);
            }
        }
    }
};
```

## Approach 2: Breadth-First Search (BFS)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <vector>
#include <queue>

class Solution {
public:
    int findCircleNum(std::vector<std::vector<int>>& isConnected) {
        int n = isConnected.size();
        std::vector<bool> visited(n, false);
        int provinces = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                bfs(isConnected, visited, i);
                provinces++;
            }
        }

        return provinces;
    }

private:
    void bfs(std::vector<std::vector<int>>& isConnected, std::vector<bool>& visited, int i) {
        std::queue<int> queue;
        queue.push(i);  // Start BFS with city i
        visited[i] = true;  // Mark as visited

        while (!queue.empty()) {
            int city = queue.front();
            queue.pop();
            for (int j = 0; j < isConnected.size(); j++) {
                // Enqueue connected and unvisited cities
                if (isConnected[city][j] == 1 && !visited[j]) {
                    queue.push(j);
                    visited[j] = true;
                }
            }
        }
    }
};
```

## Approach 3: Union-Find

### Solution
```cpp
// Time Complexity: O(n^2 * α(n))
// Space Complexity: O(n)
#include <vector>

class UnionFind {
private:
    std::vector<int> parent;
    int count; // Number of provinces

public:
    UnionFind(int n) {
        parent.resize(n);
        for (int i = 0; i < n; i++) {
            parent[i] = i; // Initial root is itself
        }
        count = n;
    }

    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }

    void unionSet(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            parent[rootX] = rootY; // Union by root
            count--;
        }
    }

    int getCount() {
        return count;
    }
};

class Solution {
public:
    int findCircleNum(std::vector<std::vector<int>>& isConnected) {
        int n = isConnected.size();
        UnionFind uf(n);

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (isConnected[i][j] == 1) {
                    uf.unionSet(i, j); // Union the connected cities
                }
            }
        }

        return uf.getCount(); // Return the number of provinces
    }
};
```

