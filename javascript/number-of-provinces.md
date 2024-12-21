# [Leetcode 547: Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Table of Contents
- [Approach 1: Depth First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Union Find (Disjoint Set Union)](#approach-2-union-find-disjoint-set-union)

---

## Approach 1: Depth First Search (DFS)

### Intuition
The problem can be visualized as a graph, where each city is a node and a direct connection between cities is an edge. The task is to find the number of connected components (provinces) in this graph. A DFS initiated from each unvisited node can help us explore all the connected nodes, marking them as visited, thereby forming a single province. We repeat this process to count all unique provinces.

### Algorithm
1. Initialize a `visited` array to keep track of which cities have been visited.
2. Iterate over each city. If a city hasn't been visited, perform a DFS from that city and increment the province count.
3. During the DFS, mark the city as visited and recursively visit all its directly connected cities.

### Time Complexity
- O(n^2), where n is the number of cities. In the worst case, we may have to visit every node and every connection.

### Space Complexity
- O(n), to maintain the visited array and recursion stack in DFS.

```javascript
/**
 * @param {number[][]} isConnected
 * @return {number}
 */
var findCircleNum = function(isConnected) {
    const n = isConnected.length;
    const visited = new Array(n).fill(false);

    const dfs = (i) => {
        visited[i] = true;
        for (let j = 0; j < n; j++) {
            // If a direct connection exists and the city has not been visited
            if (isConnected[i][j] === 1 && !visited[j]) {
                dfs(j);
            }
        }
    };

    let provinceCount = 0;
    for (let i = 0; i < n; i++) {
        if (!visited[i]) {
            dfs(i);
            provinceCount++; // New province found
        }
    }

    return provinceCount;
};
```

---

## Approach 2: Union Find (Disjoint Set Union)

### Intuition
Union-Find is an efficient algorithm to manage a partition of a set into disjoint (non-overlapping) subsets. Each time we find a connection between two cities, we unite them by updating their root parents. In the end, the number of unique root parents will be equal to the number of provinces.

### Algorithm
1. Initialize a `parent` array. Initially, each city is its own parent.
2. Define `find()` function, to find the root parent of a city with path compression.
3. Define `union()` function, to connect two cities, effectively finding their root parents and making them the same.
4. Iterate through all connections, union the nodes if they are connected.
5. Count the distinct root parents at the end as the number of provinces.

### Time Complexity
- O(n^2 * α(n)), where α is the Inverse Ackermann function, a very slow-growing function, making the Union-Find operations almost constant time.

### Space Complexity
- O(n), to store the parent for each city.

```javascript
/**
 * @param {number[][]} isConnected
 * @return {number}
 */
var findCircleNum = function(isConnected) {
    const n = isConnected.length;
    const parent = new Array(n).fill(0).map((_, index) => index);

    const find = (x) => {
        if (parent[x] !== x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    };

    const union = (x, y) => {
        const rootX = find(x);
        const rootY = find(y);
        if (rootX !== rootY) {
            parent[rootY] = rootX; // Union the sets
        }
    };

    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            // Connect if directly connected
            if (isConnected[i][j] === 1) {
                union(i, j);
            }
        }
    }

    let provinces = 0;
    for (let i = 0; i < n; i++) {
        if (parent[i] === i) {
            provinces++; // Count unique root parents
        }
    }

    return provinces;
};
```

Each approach is designed to efficiently handle the problem of determining the number of disjoint sets (provinces) from the given connections among the cities. The choice between DFS and Union-Find can depend on preference, as both are suitable for this problem size.

