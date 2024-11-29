# 802. [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

## Approach 1: Depth First Search (DFS)

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
        int[] state = new int[n];
        List<Integer> safeNodes = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            if (dfs(i, graph, state)) {
                safeNodes.add(i);
            }
        }
        return safeNodes;
    }

    private boolean dfs(int node, int[][] graph, int[] state) {
        if (state[node] != 0) {
            return state[node] == 2;
        }
        state[node] = 1;
        for (int next : graph[node]) {
            if (!dfs(next, graph, state)) {
                return false;
            }
        }
        state[node] = 2;
        return true;
    }
}
```

javascript
```javascript
// Time Complexity: O(V + E)
// Space Complexity: O(V)
var eventualSafeNodes = function(graph) {
    const n = graph.length;
    const state = new Array(n).fill(0);
    const safeNodes = [];

    for (let i = 0; i < n; i++) {
        if (dfs(i, graph, state)) {
            safeNodes.push(i);
        }
    }
    return safeNodes;
};

const dfs = (node, graph, state) => {
    if (state[node] !== 0) {
        return state[node] === 2;
    }
    state[node] = 1;
    for (const next of graph[node]) {
        if (!dfs(next, graph, state)) {
            return false;
        }
    }
    state[node] = 2;
    return true;
};
```

## Approach 2: Topological Sorting

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
            for (int j : graph[i]) {
                reverseGraph.get(j).add(i);
            }
            outDegree[i] = graph[i].length;
        }
        
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (outDegree[i] == 0) {
                queue.add(i);
            }
        }
        
        List<Integer> safeNodes = new ArrayList<>();
        
        while (!queue.isEmpty()) {
            int node = queue.poll();
            safeNodes.add(node);
            for (int prev : reverseGraph.get(node)) {
                outDegree[prev]--;
                if (outDegree[prev] == 0) {
                    queue.add(prev);
                }
            }
        }
        
        safeNodes.sort(null);
        return safeNodes;
    }
}
```

javascript
```javascript
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
var eventualSafeNodes = function(graph) {
    const n = graph.length;
    const reverseGraph = Array.from({ length: n }, () => []);
    const outDegree = new Array(n).fill(0);

    for (let i = 0; i < n; i++) {
        for (const j of graph[i]) {
            reverseGraph[j].push(i);
        }
        outDegree[i] = graph[i].length;
    }

    const queue = [];
    for (let i = 0; i < n; i++) {
        if (outDegree[i] === 0) {
            queue.push(i);
        }
    }

    const safeNodes = [];

    while (queue.length > 0) {
        const node = queue.shift();
        safeNodes.push(node);
        for (const prev of reverseGraph[node]) {
            outDegree[prev]--;
            if (outDegree[prev] === 0) {
                queue.push(prev);
            }
        }
    }

    return safeNodes.sort((a, b) => a - b);
};
```

