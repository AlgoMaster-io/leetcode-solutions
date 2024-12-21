# [Leetcode 778: Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)

## Table of Contents
1. [Approach 1: Brute Force - DFS with Backtracking](#approach-1-brute-force-dfs-with-backtracking)
2. [Approach 2: Union-Find](#approach-2-union-find)
3. [Approach 3: Min-Heap + Dijkstra's inspired](#approach-3-min-heap-dijkstras-inspired)

## Approach 1: Brute Force - DFS with Backtracking

### Intuition:
The problem asks you to determine the minimum time `T` such that you can swim from the top left to the bottom right corner of a grid. A naive solution is to simulate all possible paths using DFS, attempting to find the earliest time that allows us to reach the endpoint. Essentially, we can consider all grid cells with a value less than or equal to `T` as passable and verify if there exists a path.

### Code:
```typescript
function swimInWater(grid: number[][]): number {
    const n = grid.length;
    const directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];
    
    const isValid = (x: number, y: number, visited: boolean[][]): boolean => {
        return x >= 0 && y >= 0 && x < n && y < n && !visited[x][y];
    };

    const canSwim = (t: number, visited: boolean[][]): boolean => {
        const stack: [number, number][] = [[0, 0]];
        visited[0][0] = true;

        while (stack.length) {
            const [x, y] = stack.pop()!;
            if (x === n - 1 && y === n - 1) return true;

            for (const [dx, dy] of directions) {
                const nx = x + dx;
                const ny = y + dy;

                if (isValid(nx, ny, visited) && grid[nx][ny] <= t) {
                    visited[nx][ny] = true;
                    stack.push([nx, ny]);
                }
            }
        }
        return false;
    };

    let t = 0;
    while (true) {
        const visited = Array.from({ length: n }, () => Array(n).fill(false));
        if (canSwim(t, visited)) return t;
        t += 1;
    }
}

```

### Complexity Analysis:
- **Time Complexity:** \(O(N^4)\), where \(N\) is the dimension of the grid. The DFS may explore each cell extensively up to the maximum possible height increment of `max grid[i][j]`.
- **Space Complexity:** \(O(N^2)\) to maintain the `visited` matrix.

## Approach 2: Union-Find

### Intuition:
A more structured approach to handle connectivity in a grid is using the Union-Find structure. By gradually "flooding" the grid in non-decreasing order of heights, we can union the connected components that are passable under the current water level and check when the start and end are connected.

### Code:
```typescript
class UnionFind {
    parent: number[];
    rank: number[];

    constructor(size: number) {
        this.parent = Array.from({ length: size }, (_, idx) => idx);
        this.rank = Array(size).fill(1);
    }

    find(x: number): number {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]);
        }
        return this.parent[x];
    }

    unite(x: number, y: number): void {
        const rootX = this.find(x);
        const rootY = this.find(y);

        if (rootX !== rootY) {
            if (this.rank[rootX] > this.rank[rootY]) {
                this.parent[rootY] = rootX;
            } else if (this.rank[rootX] < this.rank[rootY]) {
                this.parent[rootX] = rootY;
            } else {
                this.parent[rootY] = rootX;
                this.rank[rootX] += 1;
            }
        }
    }
}

function swimInWater(grid: number[][]): number {
    const n = grid.length;
    const dir = [[1, 0], [0, 1]];
    const positions = grid
        .map((row, x) => row.map((val, y) => ({ val, pos: x * n + y })))
        .reduce((acc, curr) => acc.concat(curr), [])
        .sort((a, b) => a.val - b.val);

    const uf = new UnionFind(n * n);

    for (const { pos, val } of positions) {
        const x = Math.floor(pos / n);
        const y = pos % n;

        for (const [dx, dy] of dir) {
            const nx = x + dx;
            const ny = y + dy;
            if (nx < n && ny < n && grid[nx][ny] <= val) {
                uf.unite(pos, nx * n + ny);
            }
        }
        
        if (uf.find(0) === uf.find(n * n - 1)) {
            return val;
        }
    }
    return -1;
}
```

### Complexity Analysis:
- **Time Complexity:** \(O(N^2 \log N)\), mostly from sorting the positions.
- **Space Complexity:** \(O(N^2)\) for the Union-Find structure.

## Approach 3: Min-Heap + Dijkstra's inspired

### Intuition:
This solution parallels Dijkstra’s shortest path algorithm. We can think of the problem as expanding our "reachable water level" until the destination is within it. By using a min-heap to always explore the lowest possible unvisited cell, we can efficiently determine the minimum water level required to traverse to the endpoint.

### Code:
```typescript
function swimInWater(grid: number[][]): number {
    const n = grid.length;
    const directions: number[][] = [[1, 0], [-1, 0], [0, 1], [0, -1]];
    const minHeap: [number, number, number][] = [[grid[0][0], 0, 0]];
    const visited: boolean[][] = Array.from({ length: n }, () => Array(n).fill(false));
    visited[0][0] = true;

    let waterLevel = 0;

    while (minHeap.length) {
        // Pop element with minimum value
        let [currentHeight, x, y] = minHeap.shift()!;
        waterLevel = Math.max(waterLevel, currentHeight);

        if (x === n - 1 && y === n - 1) return waterLevel;

        for (const [dx, dy] of directions) {
            const nx = x + dx;
            const ny = y + dy;

            if (nx >= 0 && ny >= 0 && nx < n && ny < n && !visited[nx][ny]) {
                visited[nx][ny] = true;
                minHeap.push([grid[nx][ny], nx, ny]);
            }
        }

        // Maintain the heap property
        minHeap.sort(([h1], [h2]) => h1 - h2);
    }

    return -1;
}
```

### Complexity Analysis:
- **Time Complexity:** \(O(N^2 \log N)\) due to heap operations over \(N^2\) cells.
- **Space Complexity:** \(O(N^2)\) for the visited matrix and min-heap. 

This solution leverages the minimum priority queue to always ensure minimal exploration of the grid, akin to expanding the flooded area optimally on each step, mirroring a breadth-first search pattern.

