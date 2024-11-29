# 1584. [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

## Approach 1: Prim's Algorithm 

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n)

function minCostConnectPoints(points: number[][]): number {
    const n = points.length;
    const inMST = new Array(n).fill(false);
    const minHeap = new MinPriorityQueue({ priority: (a) => a[0] });

    minHeap.enqueue([0, 0]); // (cost, point)

    let totalCost = 0;
    let edgesUsed = 0;

    while (edgesUsed < n) {
        const { element: [cost, u] } = minHeap.dequeue();

        if (inMST[u]) continue; // Skip if already included in the MST

        inMST[u] = true;
        totalCost += cost;
        edgesUsed++;

        for (let v = 0; v < n; v++) {
            if (!inMST[v]) {
                const dist = Math.abs(points[u][0] - points[v][0]) + Math.abs(points[u][1] - points[v][1]);
                minHeap.enqueue([dist, v]);
            }
        }
    }

    return totalCost;
}
```

## Approach 2: Kruskal's Algorithm with Union-Find

### Solution
typescript
```typescript
// Time Complexity: O(n^2 log n)
// Space Complexity: O(n^2)

class UnionFind {
    private parent: number[];
    private rank: number[];

    constructor(n: number) {
        this.parent = Array.from({ length: n }, (_, i) => i);
        this.rank = new Array(n).fill(1);
    }

    find(x: number): number {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]); // Path compression
        }
        return this.parent[x];
    }

    union(x: number, y: number): boolean {
        const rootX = this.find(x);
        const rootY = this.find(y);

        if (rootX !== rootY) {
            if (this.rank[rootX] > this.rank[rootY]) {
                this.parent[rootY] = rootX;
            } else if (this.rank[rootX] < this.rank[rootY]) {
                this.parent[rootX] = rootY;
            } else {
                this.parent[rootY] = rootX;
                this.rank[rootX]++;
            }
            return true;
        }

        return false;
    }
}

function minCostConnectPointsKruskal(points: number[][]): number {
    const n = points.length;
    const edges = new MinPriorityQueue({ priority: (a) => a[2] });

    // Calculate all possible edges and their costs
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            const cost = Math.abs(points[i][0] - points[j][0]) + Math.abs(points[i][1] - points[j][1]);
            edges.enqueue([i, j, cost]);
        }
    }

    const uf = new UnionFind(n);
    let totalCost = 0;
    let edgesUsed = 0;

    while (edgesUsed < n - 1) {
        const { element: [u, v, cost] } = edges.dequeue();

        if (uf.union(u, v)) {
            totalCost += cost;
            edgesUsed++;
        }
    }

    return totalCost;
}
```

