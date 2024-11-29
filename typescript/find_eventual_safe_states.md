# 802. [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

## Approach 1: Depth First Search (DFS) and Coloring

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        int[] color = new int[n]; // 0=unknown, 1=safe, 2=unsafe
        List<Integer> safeNodes = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            if (dfs(graph, color, i)) {
                safeNodes.add(i);
            }
        }

        return safeNodes;
    }

    private boolean dfs(int[][] graph, int[] color, int node) {
        if (color[node] != 0) {
            return color[node] == 1;
        }

        color[node] = 2; // Mark as unsafe temporally
        for (int neighbour : graph[node]) {
            if (!dfs(graph, color, neighbour)) {
                return false;
            }
        }

        color[node] = 1; // Mark as safe
        return true;
    }
}
```

typescript
```typescript
// Time Complexity: O(V + E)
// Space Complexity: O(V)
function eventualSafeNodes(graph: number[][]): number[] {
    const n = graph.length;
    const color = new Array(n).fill(0); // 0=unknown, 1=safe, 2=unsafe
    const safeNodes: number[] = [];

    for (let i = 0; i < n; i++) {
        if (dfs(graph, color, i)) {
            safeNodes.push(i);
        }
    }

    return safeNodes;
}

function dfs(graph: number[][], color: number[], node: number): boolean {
    if (color[node] !== 0) {
        return color[node] === 1;
    }

    color[node] = 2; // Mark as unsafe temporally
    for (const neighbour of graph[node]) {
        if (!dfs(graph, color, neighbour)) {
            return false;
        }
    }

    color[node] = 1; // Mark as safe
    return true;
}
```

## Approach 2: Topological Sort using Reverse Graph

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        List<List<Integer>> reverseGraph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            reverseGraph.add(new ArrayList<>());
        }
        
        int[] outDegree = new int[n];
        for (int i = 0; i < n; i++) {
            for (int neighbor : graph[i]) {
                reverseGraph.get(neighbor).add(i);
            }
            outDegree[i] = graph[i].length;
        }

        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (outDegree[i] == 0) {
                queue.offer(i);
            }
        }

        List<Integer> safeNodes = new ArrayList<>();
        while (!queue.isEmpty()) {
            int node = queue.poll();
            safeNodes.add(node);
            for (int neighbor : reverseGraph.get(node)) {
                outDegree[neighbor]--;
                if (outDegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }
        
        safeNodes.sort(Integer::compareTo);
        return safeNodes;
    }
}
```

typescript
```typescript
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
function eventualSafeNodes(graph: number[][]): number[] {
    const n = graph.length;
    const reverseGraph: number[][] = Array.from({ length: n }, () => []);
    const outDegree = new Array(n).fill(0);

    for (let i = 0; i < n; i++) {
        for (const neighbor of graph[i]) {
            reverseGraph[neighbor].push(i);
        }
        outDegree[i] = graph[i].length;
    }

    const queue: number[] = [];
    for (let i = 0; i < n; i++) {
        if (outDegree[i] === 0) {
            queue.push(i);
        }
    }

    const safeNodes: number[] = [];
    while (queue.length > 0) {
        const node = queue.shift()!;
        safeNodes.push(node);
        for (const neighbor of reverseGraph[node]) {
            outDegree[neighbor]--;
            if (outDegree[neighbor] === 0) {
                queue.push(neighbor);
            }
        }
    }

    return safeNodes.sort((a, b) => a - b);
}
```


