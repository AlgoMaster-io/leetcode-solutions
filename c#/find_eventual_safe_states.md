# 802. [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

## Approach 1: Depth First Search with Cycle Detection

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        int[] state = new int[n]; // 0 = unvisited, 1 = visiting, 2 = safe
        List<Integer> safeNodes = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            if (dfs(i, graph, state)) {
                safeNodes.add(i);
            }
        }

        return safeNodes;
    }

    private boolean dfs(int node, int[][] graph, int[] state) {
        if (state[node] > 0) {
            return state[node] == 2;
        }

        state[node] = 1; // Mark current node as visiting
        for (int neighbor : graph[node]) {
            if (!dfs(neighbor, graph, state)) {
                return false;
            }
        }

        state[node] = 2; // Mark current node as safe
        return true;
    }
}
```

csharp
```csharp
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
using System;
using System.Collections.Generic;

public class Solution {
    public IList<int> EventualSafeNodes(int[][] graph) {
        int n = graph.Length;
        int[] state = new int[n]; // 0 = unvisited, 1 = visiting, 2 = safe
        List<int> safeNodes = new List<int>();

        for (int i = 0; i < n; i++) {
            if (Dfs(i, graph, state)) {
                safeNodes.Add(i);
            }
        }

        return safeNodes;
    }

    private bool Dfs(int node, int[][] graph, int[] state) {
        if (state[node] > 0) {
            return state[node] == 2;
        }

        state[node] = 1; // Mark current node as visiting
        foreach (int neighbor in graph[node]) {
            if (!Dfs(neighbor, graph, state)) {
                return false;
            }
        }

        state[node] = 2; // Mark current node as safe
        return true;
    }
}
```

## Approach 2: Topological Sort

### Solution
java
```java
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
import java.util.ArrayList;
import java.util.Arrays;
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
                outDegree[i]++;
            }
        }

        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (outDegree[i] == 0) {
                queue.offer(i);
            }
        }

        boolean[] safe = new boolean[n];
        while (!queue.isEmpty()) {
            int node = queue.poll();
            safe[node] = true;
            for (int neighbor : reverseGraph.get(node)) {
                outDegree[neighbor]--;
                if (outDegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }

        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (safe[i]) {
                result.add(i);
            }
        }

        return result;
    }
}
```

csharp
```csharp
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
using System;
using System.Collections.Generic;

public class Solution {
    public IList<int> EventualSafeNodes(int[][] graph) {
        int n = graph.Length;
        List<List<int>> reverseGraph = new List<List<int>>();
        for (int i = 0; i < n; i++) {
            reverseGraph.Add(new List<int>());
        }

        int[] outDegree = new int[n];
        for (int i = 0; i < n; i++) {
            foreach (int j in graph[i]) {
                reverseGraph[j].Add(i);
                outDegree[i]++;
            }
        }

        Queue<int> queue = new Queue<int>();
        for (int i = 0; i < n; i++) {
            if (outDegree[i] == 0) {
                queue.Enqueue(i);
            }
        }

        bool[] safe = new bool[n];
        while (queue.Count > 0) {
            int node = queue.Dequeue();
            safe[node] = true;
            foreach (int neighbor in reverseGraph[node]) {
                outDegree[neighbor]--;
                if (outDegree[neighbor] == 0) {
                    queue.Enqueue(neighbor);
                }
            }
        }

        List<int> result = new List<int>();
        for (int i = 0; i < n; i++) {
            if (safe[i]) {
                result.Add(i);
            }
        }

        return result;
    }
}
```

