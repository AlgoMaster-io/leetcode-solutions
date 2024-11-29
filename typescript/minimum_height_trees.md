# 310. [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

## Approach 1: Topological Sort (BFS for Heights)

### Solution
java
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.*;

public class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if (n == 1) return Collections.singletonList(0);
        
        List<Set<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new HashSet<>());
        }
        
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }
        
        List<Integer> leaves = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (graph.get(i).size() == 1) {
                leaves.add(i);
            }
        }
        
        while (n > 2) {
            n -= leaves.size();
            List<Integer> newLeaves = new ArrayList<>();
            for (int leaf : leaves) {
                int neighbor = graph.get(leaf).iterator().next();
                graph.get(neighbor).remove(leaf);
                if (graph.get(neighbor).size() == 1) {
                    newLeaves.add(neighbor);
                }
            }
            leaves = newLeaves;
        }
        
        return leaves;
    }
}
```

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function findMinHeightTrees(n: number, edges: number[][]): number[] {
    if (n === 1) return [0];

    const graph: Set<number>[] = Array.from({ length: n }, () => new Set());

    for (const [u, v] of edges) {
        graph[u].add(v);
        graph[v].add(u);
    }

    let leaves: number[] = [];
    for (let i = 0; i < n; i++) {
        if (graph[i].size === 1) {
            leaves.push(i);
        }
    }

    while (n > 2) {
        n -= leaves.length;
        const newLeaves: number[] = [];
        for (const leaf of leaves) {
            const neighbor = graph[leaf].values().next().value;
            graph[neighbor].delete(leaf);
            if (graph[neighbor].size === 1) {
                newLeaves.push(neighbor);
            }
        }
        leaves = newLeaves;
    }

    return leaves;
}
```

The TypeScript solution preserves the original logic but adapts it to TypeScript's strict type system, ensuring that sets and arrays are properly initialized and iterated over.

