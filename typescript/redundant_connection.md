# 684. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approach 1: Union-Find Algorithm

### Solution
```typescript
// Time Complexity: O(n * α(n)), where α is the inverse Ackermann function
// Space Complexity: O(n)
class Solution {
    findRedundantConnection(edges: number[][]): number[] {
        const n = edges.length;
        const parent: number[] = Array(n + 1).fill(0).map((_, i) => i);
        const rank: number[] = Array(n + 1).fill(1);

        for (const [u, v] of edges) {
            if (this.union(u, v, parent, rank)) {
                return [u, v];
            }
        }

        return [];
    }

    private find(node: number, parent: number[]): number {
        if (parent[node] !== node) {
            parent[node] = this.find(parent[node], parent);
        }
        return parent[node];
    }

    private union(u: number, v: number, parent: number[], rank: number[]): boolean {
        const rootU = this.find(u, parent);
        const rootV = this.find(v, parent);

        if (rootU === rootV) {
            return true;
        }

        if (rank[rootU] > rank[rootV]) {
            parent[rootV] = rootU;
        } else if (rank[rootU] < rank[rootV]) {
            parent[rootU] = rootV;
        } else {
            parent[rootV] = rootU;
            rank[rootU]++;
        }

        return false;
    }
}
```

## Approach 2: DFS to Detect Cycle

### Solution
```typescript
// Time Complexity: O(n^2), in the worst case for a dense graph
// Space Complexity: O(n)
class Solution {
    findRedundantConnection(edges: number[][]): number[] {
        const n = edges.length;
        const graph: number[][] = Array.from({ length: n + 1 }, () => []);

        for (const [u, v] of edges) {
            if (this.hasCycleDFS(graph, u, v, new Array(n + 1).fill(false))) {
                return [u, v];
            }
            graph[u].push(v);
            graph[v].push(u);
        }

        return [];
    }

    private hasCycleDFS(graph: number[][], source: number, target: number, visited: boolean[]): boolean {
        if (visited[source]) return false;

        visited[source] = true;

        for (const neighbor of graph[source]) {
            if (neighbor === target) continue;
            if (visited[neighbor] || this.hasCycleDFS(graph, neighbor, target, visited)) {
                return true;
            }
        }

        return false;
    }
}
```

