# 684. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approach 1: Union-Find Algorithm

### Solution
```javascript
// Time Complexity: O(n * α(n)), where α is the inverse Ackermann function
// Space Complexity: O(n)
class Solution {
    findRedundantConnection(edges) {
        const n = edges.length;
        const parent = Array(n + 1).fill(0).map((_, index) => index);
        const rank = Array(n + 1).fill(1);

        const find = (node) => {
            if (parent[node] !== node) {
                parent[node] = find(parent[node]);
            }
            return parent[node];
        };

        const union = (u, v) => {
            const rootU = find(u);
            const rootV = find(v);

            if (rootU === rootV) {
                return true; // u and v are already connected
            }

            // Union by rank
            if (rank[rootU] > rank[rootV]) {
                parent[rootV] = rootU;
            } else if (rank[rootU] < rank[rootV]) {
                parent[rootU] = rootV;
            } else {
                parent[rootV] = rootU;
                rank[rootU]++;
            }
            
            return false;
        };

        for (let edge of edges) {
            if (union(edge[0], edge[1])) {
                return edge;
            }
        }

        return []; // If no redundant connection is found
    }
}
```

## Approach 2: DFS to Detect Cycle

### Solution
```javascript
// Time Complexity: O(n^2), in the worst case for a dense graph
// Space Complexity: O(n)
class Solution {
    findRedundantConnection(edges) {
        const n = edges.length;
        const graph = Array.from({ length: n + 1 }, () => []);

        const hasCycleDFS = (graph, source, target, visited) => {
            if (visited[source]) return false;

            visited[source] = true;

            for (let neighbor of graph[source]) {
                if (neighbor === target) continue;
                if (visited[neighbor] || hasCycleDFS(graph, neighbor, target, visited)) {
                    return true;
                }
            }

            return false;
        };

        for (let edge of edges) {
            const visited = Array(n + 1).fill(false);
            if (hasCycleDFS(graph, edge[0], edge[1], visited)) {
                return edge;
            }
            graph[edge[0]].push(edge[1]);
            graph[edge[1]].push(edge[0]);
        }

        return []; // If no redundant connection is found
    }
}
```

