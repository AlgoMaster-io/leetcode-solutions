# 310. [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

## Approach 1: Topological Sorting (Kahn's Algorithm) with Degree Count

### Solution
java
```java
// Time Complexity: O(n)
// Space Complexity: O(n)

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if (n == 1) {
            List<Integer> result = new ArrayList<>();
            result.add(0);
            return result;
        }

        List<List<Integer>> adjList = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adjList.add(new ArrayList<>());
        }

        int[] degree = new int[n];
        for (int[] edge : edges) {
            adjList.get(edge[0]).add(edge[1]);
            adjList.get(edge[1]).add(edge[0]);
            degree[edge[0]]++;
            degree[edge[1]]++;
        }

        Queue<Integer> leaves = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (degree[i] == 1) {
                leaves.add(i);
            }
        }

        while (n > 2) {
            int size = leaves.size();
            n -= size;
            for (int i = 0; i < size; i++) {
                int leaf = leaves.poll();
                for (int neighbor : adjList.get(leaf)) {
                    degree[neighbor]--;
                    if (degree[neighbor] == 1) {
                        leaves.add(neighbor);
                    }
                }
            }
        }

        return new ArrayList<>(leaves);
    }
}
```

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)

function findMinHeightTrees(n, edges) {
    if (n === 1) {
        return [0];
    }

    const adjList = Array.from({ length: n }, () => []);
    const degree = Array(n).fill(0);

    for (const [u, v] of edges) {
        adjList[u].push(v);
        adjList[v].push(u);
        degree[u]++;
        degree[v]++;
    }

    const leaves = [];
    for (let i = 0; i < n; i++) {
        if (degree[i] === 1) {
            leaves.push(i);
        }
    }

    while (n > 2) {
        const size = leaves.length;
        n -= size;
        for (let i = 0; i < size; i++) {
            const leaf = leaves.shift();
            for (const neighbor of adjList[leaf]) {
                degree[neighbor]--;
                if (degree[neighbor] === 1) {
                    leaves.push(neighbor);
                }
            }
        }
    }

    return leaves;
}
```

