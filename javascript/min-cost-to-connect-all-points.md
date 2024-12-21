## [Leetcode 1584: Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

### Approaches:
1. [Prim's Algorithm (Basic)](#prim-algorithm-basic)
2. [Kruskal's Algorithm with Union-Find (Optimized)](#kruskal-optimized)

---

### 1. Prim's Algorithm (Basic)

**Intuition**:
The problem can be framed as finding the Minimum Spanning Tree (MST) of a graph where each point is a vertex and the cost to connect any two points is their Manhattan distance. Prim's algorithm is a classic method to find the MST of a graph by incrementally building the MST set, starting with an arbitrary point and expanding it by adding the lowest cost edge that connects any point outside the MST to a point inside the MST.

**Steps**:
1. Start with an arbitrary point.
2. Maintain a priority queue (or a min-heap) to pick the next minimum weight edge that connects the MST set with a new vertex.
3. Add the selected vertex to the MST set.
4. Repeat the step until all points are included in the MST.

**Complexity**:  
- Time Complexity: O(N^2 log N) where N is the number of points. This is primarily due to the update operation in the priority queue for each vertex.
- Space Complexity: O(N^2) due to the storage of all possible edges in a graph.

```javascript
function minCostConnectPoints(points) {
    const n = points.length;
    // Maintain a min-heap priority queue to pick the smallest edge
    const minHeap = new MinPriorityQueue({ priority: x => x[0] });
    // Visited array to keep track of included points in MST
    const visited = Array(n).fill(false);
    minHeap.enqueue([0, 0]); // Start with the first point with cost 0
    let totalCost = 0;
    let count = 0;

    // Run until we include all points in the MST
    while (count < n) {
        const [cost, currentPoint] = minHeap.dequeue().element;

        // If the current point has been visited, continue to the next
        if (visited[currentPoint]) continue;

        // Mark it as visited and add its cost to the total
        visited[currentPoint] = true;
        totalCost += cost;
        count++;

        // Check all points to find the next minimum cost edge
        for (let nextPoint = 0; nextPoint < n; nextPoint++) {
            if (!visited[nextPoint]) {
                const [x1, y1] = points[currentPoint];
                const [x2, y2] = points[nextPoint];
                const newCost = Math.abs(x1 - x2) + Math.abs(y1 - y2);
                minHeap.enqueue([newCost, nextPoint]);
            }
        }
    }

    return totalCost;
}
```

---

### 2. Kruskal's Algorithm with Union-Find (Optimized)

**Intuition**:
Kruskal’s algorithm is another approach for finding the MST. It involves sorting all the edges and picking those which form the minimum weight tree without forming a cycle. Union-Find (or Disjoint Set Union, DSU) helps in efficiently detecting cycles while adding these edges.

**Steps**:
1. Calculate the Manhattan distance for every possible pair of points, resulting in an edge list.
2. Sort the edges by their weights.
3. Use union-find to keep track of connected components; add edges from the sorted list until all points are connected.

**Complexity**:  
- Time Complexity: O(E log E + E α(N)) where E is the number of edges, and α is the inverse Ackermann function arising from the union-find operations. In this case, E = O(N^2) as we consider all pairs of points.
- Space Complexity: O(N) for the union-find parent and rank arrays.

```javascript
function minCostConnectPoints(points) {
    const n = points.length;
    const edges = [];

    // Build the edge list with distances
    for (let i = 0; i < n - 1; i++) {
        for (let j = i + 1; j < n; j++) {
            const weight = Math.abs(points[i][0] - points[j][0]) + Math.abs(points[i][1] - points[j][1]);
            edges.push([weight, i, j]);
        }
    }

    // Sort edges by distances (smallest first)
    edges.sort((a, b) => a[0] - b[0]);

    const parent = Array(n).fill(0).map((_, index) => index);
    const rank = Array(n).fill(1);

    // Find function using path compression
    function find(x) {
        if (parent[x] !== x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    // Union function using union by rank
    function union(x, y) {
        const rootX = find(x);
        const rootY = find(y);
        if (rootX !== rootY) {
            if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
            return true;
        }
        return false;
    }

    let totalCost = 0;
    let edgesUsed = 0;

    for (const [weight, u, v] of edges) {
        if (union(u, v)) {
            totalCost += weight;
            edgesUsed++;
            if (edgesUsed === n - 1) break;
        }
    }

    return totalCost;
}
```

---

