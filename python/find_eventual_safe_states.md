# 802. [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

## Approach 1: Using Depth-First Search (DFS) with Coloring Method

### Solution
java
```java
// Time Complexity: O(n + e) - Where n is the number of nodes and e is the number of edges
// Space Complexity: O(n)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        int[] colors = new int[n]; // 0 = unvisited, 1 = visiting, 2 = safe
        List<Integer> result = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            if (isSafeNode(graph, colors, i)) {
                result.add(i);
            }
        }
        return result;
    }
    
    private boolean isSafeNode(int[][] graph, int[] colors, int node) {
        if (colors[node] > 0) {
            return colors[node] == 2;
        }
        colors[node] = 1; // Mark node as visiting
        for (int next : graph[node]) {
            if (!isSafeNode(graph, colors, next)) {
                return false;
            }
        }
        colors[node] = 2; // Mark node as safe
        return true;
    }
}
```


### Solution
python
```python
# Time Complexity: O(n + e) - Where n is the number of nodes and e is the number of edges
# Space Complexity: O(n)
from typing import List

class Solution:
    def eventualSafeNodes(self, graph: List[List[int]]) -> List[int]:
        n = len(graph)
        colors = [0] * n  # 0 = unvisited, 1 = visiting, 2 = safe
        result = []

        def is_safe_node(node):
            if colors[node] > 0:
                return colors[node] == 2
            colors[node] = 1  # Mark node as visiting
            for next_node in graph[node]:
                if not is_safe_node(next_node):
                    return False
            colors[node] = 2  # Mark node as safe
            return True

        for i in range(n):
            if is_safe_node(i):
                result.append(i)

        return result
```


## Approach 2: Using Topological Sorting (Kahn's Algorithm)

### Solution
java
```java
// Time Complexity: O(n + e) - Where n is the number of nodes and e is the number of edges
// Space Complexity: O(n + e)
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        List<List<Integer>> reverseGraph = new ArrayList<>();
        int[] outDegrees = new int[n];
        
        for (int i = 0; i < n; i++) {
            reverseGraph.add(new ArrayList<>());
        }
        
        for (int u = 0; u < n; u++) {
            for (int v : graph[u]) {
                reverseGraph.get(v).add(u);
                outDegrees[u]++;
            }
        }
        
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (outDegrees[i] == 0) {
                queue.add(i);
            }
        }
        
        List<Integer> result = new ArrayList<>();
        while (!queue.isEmpty()) {
            int u = queue.poll();
            result.add(u);
            for (int v : reverseGraph.get(u)) {
                outDegrees[v]--;
                if (outDegrees[v] == 0) {
                    queue.add(v);
                }
            }
        }
        
        java.util.Collections.sort(result);
        return result;
    }
}
```


### Solution
python
```python
# Time Complexity: O(n + e) - Where n is the number of nodes and e is the number of edges
# Space Complexity: O(n + e)
from typing import List
from collections import deque

class Solution:
    def eventualSafeNodes(self, graph: List[List[int]]) -> List[int]:
        n = len(graph)
        reverse_graph = [[] for _ in range(n)]
        out_degrees = [0] * n

        for u in range(n):
            for v in graph[u]:
                reverse_graph[v].append(u)
                out_degrees[u] += 1

        queue = deque()
        for i in range(n):
            if out_degrees[i] == 0:
                queue.append(i)

        result = []
        while queue:
            u = queue.popleft()
            result.append(u)
            for v in reverse_graph[u]:
                out_degrees[v] -= 1
                if out_degrees[v] == 0:
                    queue.append(v)

        return sorted(result)
```

