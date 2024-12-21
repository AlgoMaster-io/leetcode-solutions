# [LeetCode 547: Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Solutions
- [Approach 1: Depth First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Union Find](#approach-2-union-find)

### Approach 1: Depth First Search (DFS)

#### Intuition
The problem gives us a matrix where `isConnected[i][j] == 1` implies city `i` is directly connected to city `j`. A province is a group of cities directly or indirectly connected. We need to find the number of such groups (provinces).

Using Depth First Search (DFS), we can explore all cities connected to a given city and mark them as visited. Each time we start a new DFS, it indicates the discovery of a new province.

#### Code
```typescript
function findCircleNum(isConnected: number[][]): number {
    const n = isConnected.length;
    const visited = new Array(n).fill(false);

    function dfs(city: number): void {
        for (let otherCity = 0; otherCity < n; otherCity++) {
            // If there's a direct connection and the city hasn't been visited
            if (isConnected[city][otherCity] === 1 && !visited[otherCity]) {
                visited[otherCity] = true;  // Mark as visited
                dfs(otherCity);  // Visit connected city
            }
        }
    }

    let provinceCount = 0;
    for (let i = 0; i < n; i++) {
        if (!visited[i]) {
            // If city i is not visited, we have a new province
            provinceCount++;
            visited[i] = true;
            dfs(i);  // Visit all cities in this province
        }
    }

    return provinceCount;
}
```

#### Time Complexity
- **O(n^2)** where `n` is the number of cities. In the worst case, we are exploring each edge of the matrix once.

#### Space Complexity
- **O(n)** due to the visited array used to keep track of visited cities.

### Approach 2: Union Find

#### Intuition
Using Union Find, we treat each city as a node and each direct connection as an edge. Our goal is to find the number of connected components in this graph. Union Find helps efficiently manage connected components and find the number of provinces.

#### Code
```typescript
function findCircleNum(isConnected: number[][]): number {
    const n = isConnected.length;
    const parent = Array.from({length: n}, (_, i) => i);

    function find(x: number): number {
        if (parent[x] !== x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    function union(x: number, y: number): void {
        const rootX = find(x);
        const rootY = find(y);
        if (rootX !== rootY) {
            parent[rootX] = rootY;  // Union the sets
        }
    }

    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            if (isConnected[i][j] === 1) {
                union(i, j);
            }
        }
    }

    const provinceSet = new Set<number>();
    for (let i = 0; i < n; i++) {
        // Find root of each city to count distinct components
        provinceSet.add(find(i));
    }

    return provinceSet.size;
}
```

#### Time Complexity
- **O(n^2)** for iterating through each pair of cities.
- With path compression and union by rank, operations are nearly constant time.

#### Space Complexity
- **O(n)** for the parent array used to track the connected components.

