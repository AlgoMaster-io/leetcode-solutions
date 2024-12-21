# [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

### Approaches:
- [Approach 1: Prim's Algorithm](#approach-1-prims-algorithm)
- [Approach 2: Kruskal's Algorithm with Union-Find](#approach-2-kruskals-algorithm-with-union-find)

## Approach 1: Prim's Algorithm

### Intuition:
Prim's Algorithm is one of the efficient methods to find the Minimum Spanning Tree (MST) of a graph. Given a set of points, we can treat them as nodes of a graph where the weight of an edge between two nodes (points) is the Manhattan distance between them.

### Solution:
1. Start with an arbitrary point, adding it to the MST.
2. Use a priority queue (min-heap) to always extend the MST using the smallest edge.
3. Add the point connected by this smallest edge into the MST, updating the cost of the MST.
4. Repeat this process until all points are included in the MST.

```typescript
function minCostConnectPoints(points: number[][]): number {
    const n = points.length;
    const minDist = new Array(n).fill(Infinity);
    const inMST = new Array(n).fill(false);
    minDist[0] = 0;
    let cost = 0;

    for (let i = 0; i < n; i++) {
        let u = -1;

        for (let j = 0; j < n; j++) {
            // Find the vertex with the smallest cost that is not yet in the MST
            if (!inMST[j] && (u === -1 || minDist[j] < minDist[u])) {
                u = j;
            }
        }

        // Include this vertex into MST
        inMST[u] = true;
        cost += minDist[u];

        // Update the distances of neighboring vertices not yet in MST
        for (let v = 0; v < n; v++) {
            const dist = Math.abs(points[u][0] - points[v][0]) + Math.abs(points[u][1] - points[v][1]);
            
            if (!inMST[v] && dist < minDist[v]) {
                minDist[v] = dist;
            }
        }
    }

    return cost;
}
```

### Time Complexity:
- O(n^2), where n is the number of points. This is due to the loop to find the minimum weight edge in the heap.

### Space Complexity:
- O(n) due to the additional arrays `minDist` and `inMST`.

## Approach 2: Kruskal's Algorithm with Union-Find

### Intuition:
Kruskal's Algorithm also finds the MST by sorting all edges first and then adding them one by one. The Union-Find (disjoint set union) data structure is used to detect cycles.

### Solution:
1. Calculate all possible edges and sort them based on weight (Manhattan distance).
2. Initialize a Union-Find structure to manage connected components.
3. Iterate through sorted edges, adding an edge to the MST if it doesn't form a cycle, and using Union-Find to check connectivity.

```typescript
class UnionFind {
    parent: number[];
    
    constructor(size: number) {
        this.parent = Array.from({ length: size }, (_, index) => index);
    }
    
    find(x: number): number {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]);
        }
        return this.parent[x];
    }
    
    union(x: number, y: number): boolean {
        const rootX = this.find(x);
        const rootY = this.find(y);
        
        if (rootX !== rootY) {
            this.parent[rootX] = rootY;
            return true;
        }
        return false;
    }
}

function minCostConnectPoints(points: number[][]): number {
    const n = points.length;
    const edges: [number, number, number][] = [];
    
    // Construct all edges with weights
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            const weight = Math.abs(points[i][0] - points[j][0]) + Math.abs(points[i][1] - points[j][1]);
            edges.push([weight, i, j]);
        }
    }

    // Sort edges by weight
    edges.sort(([w1], [w2]) => w1 - w2);

    const uf = new UnionFind(n);
    let cost = 0;
    let edgesUsed = 0;

    for (const [weight, u, v] of edges) {
        if (uf.union(u, v)) {
            cost += weight;
            edgesUsed += 1;
            if (edgesUsed === n - 1) break;
        }
    }

    return cost;
}
```

### Time Complexity:
- O(n^2 log n), primarily due to sorting the edges.

### Space Complexity:
- O(n^2), because we need to store all edges.

With these solutions, both approaches efficiently calculate the minimum cost to connect all the points based on Manhattan distances, utilizing core graph algorithms for Minimum Spanning Tree.

