# [Leetcode 684: Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approaches
- [Approach 1: Depth First Search](#approach-1-depth-first-search)
- [Approach 2: Union-Find with Path Compression](#approach-2-union-find-with-path-compression)

---

## Approach 1: Depth First Search

### Intuition
The problem states that there is a tree structure with one extra edge, which makes it a graph with a cycle. To identify this extra edge, we can use a DFS approach to detect the cycle in the undirected graph.

### Steps
1. Start with no edges in the graph.
2. Add each edge to the graph and perform a DFS from one vertex of the edge to find if there's a cycle.
3. If a cycle is detected after adding an edge, that edge is the redundant connection.
4. Return the edge that causes a cycle.

### Time Complexity
- O(N^2) in the worst case because for every edge we perform a DFS which can visit all the other edges.

### Space Complexity
- O(N) for storing the graph representation.

### Code
```typescript
function findRedundantConnection(edges: number[][]): number[] {
    const graph: Map<number, number[]> = new Map();

    const dfs = (start: number, end: number, visited: Set<number>): boolean => {
        if (start === end) return true;
        visited.add(start);

        if (graph.has(start)) {
            for (let neighbor of graph.get(start)!) {
                if (!visited.has(neighbor)) {
                    if (dfs(neighbor, end, visited)) return true;
                }
            }
        }
        return false;
    };

    for (let [u, v] of edges) {
        const visited = new Set<number>();
        // Check for cycle: If there's a path between u and v already,
        // adding this edge will form a cycle.
        if (dfs(u, v, visited)) return [u, v];

        if (!graph.has(u)) graph.set(u, []);
        if (!graph.has(v)) graph.set(v, []);
        graph.get(u)!.push(v);
        graph.get(v)!.push(u);
    }

    return [];
}
```

---

## Approach 2: Union-Find with Path Compression

### Intuition
The Union-Find data structure helps in efficiently managing connected components and detecting cycles. We can use this approach because in a tree with N nodes, there are exactly N-1 edges. By continually adding edges and merging components, a newly added edge that forms a cycle indicates that the two nodes are already connected. 

### Steps
1. Initialize a parent pointer for each node to itself and a rank array.
2. Use a union-find method to check if two nodes share the same root. If so, a cycle is detected.
3. Otherwise, perform the union operation.
4. Return the first edge that fails the union operation due to shared roots.

### Time Complexity
- O(N * α(N)) where α is the inverse Ackermann function, practically constant.

### Space Complexity
- O(N) for storing the parent and rank arrays.

### Code
```typescript
class UnionFind {
    parent: number[];
    rank: number[];

    constructor(size: number) {
        this.parent = Array(size).fill(0).map((_, index) => index);
        this.rank = Array(size).fill(1);
    }

    find(x: number): number {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]); // Path compression
        }
        return this.parent[x];
    }

    union(x: number, y: number): boolean {
        let rootX = this.find(x);
        let rootY = this.find(y);

        if (rootX === rootY) return false; // x and y are already in the same set

        // Union by rank
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
}

function findRedundantConnection(edges: number[][]): number[] {
    const uf = new UnionFind(edges.length + 1);

    for (let [u, v] of edges) {
        if (!uf.union(u, v)) {
            return [u, v];
        }
    }

    return [];
}
```

This approach is optimal in terms of both time and space efficiency and quickly identifies the redundant connection causing a cycle using a union-find structure with path compression.

