# 547. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

## Approach 1: Depth-First Search (DFS)

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
class Solution {
    findCircleNum(isConnected: number[][]): number {
        const n = isConnected.length;
        const visited = new Array<boolean>(n).fill(false);
        let provinces = 0;

        // Perform DFS for each city if not visited
        for (let i = 0; i < n; i++) {
            if (!visited[i]) {
                this.dfs(isConnected, visited, i);
                provinces++; // Each DFS completes a province
            }
        }

        return provinces;
    }

    private dfs(isConnected: number[][], visited: boolean[], i: number): void {
        visited[i] = true; // Mark the city as visited
        for (let j = 0; j < isConnected.length; j++) {
            // Visit connected cities that have not been visited
            if (isConnected[i][j] === 1 && !visited[j]) {
                this.dfs(isConnected, visited, j);
            }
        }
    }
}
```

## Approach 2: Breadth-First Search (BFS)

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
class Solution {
    findCircleNum(isConnected: number[][]): number {
        const n = isConnected.length;
        const visited = new Array<boolean>(n).fill(false);
        let provinces = 0;

        for (let i = 0; i < n; i++) {
            if (!visited[i]) {
                this.bfs(isConnected, visited, i);
                provinces++;
            }
        }

        return provinces;
    }

    private bfs(isConnected: number[][], visited: boolean[], i: number): void {
        const queue: number[] = [];
        queue.push(i); // Start BFS with city i
        visited[i] = true; // Mark as visited

        while (queue.length > 0) {
            const city = queue.shift()!;
            for (let j = 0; j < isConnected.length; j++) {
                // Enqueue connected and unvisited cities
                if (isConnected[city][j] === 1 && !visited[j]) {
                    queue.push(j);
                    visited[j] = true;
                }
            }
        }
    }
}
```

## Approach 3: Union-Find

### Solution
typescript
```typescript
// Time Complexity: O(n^2 * α(n))
// Space Complexity: O(n)
class Solution {
    findCircleNum(isConnected: number[][]): number {
        const n = isConnected.length;
        const uf = new UnionFind(n);

        for (let i = 0; i < n; i++) {
            for (let j = 0; j < n; j++) {
                if (isConnected[i][j] === 1) {
                    uf.union(i, j); // Union the connected cities
                }
            }
        }

        return uf.getCount(); // Return the number of provinces
    }
}

class UnionFind {
    private parent: number[];
    private count: number; // Number of provinces

    constructor(n: number) {
        this.parent = new Array<number>(n);
        for (let i = 0; i < n; i++) {
            this.parent[i] = i; // Initial root is itself
        }
        this.count = n;
    }

    public find(x: number): number {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]); // Path compression
        }
        return this.parent[x];
    }

    public union(x: number, y: number): void {
        const rootX = this.find(x);
        const rootY = this.find(y);
        if (rootX !== rootY) {
            this.parent[rootX] = rootY; // Union by root
            this.count--;
        }
    }

    public getCount(): number {
        return this.count;
    }
}
```

