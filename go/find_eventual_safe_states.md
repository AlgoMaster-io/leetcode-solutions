# 802. [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

## Approach 1: Depth First Search (DFS) with Memorization

### Solution
java
```java
// Time Complexity: O(V + E), where V is the number of vertices and E is the number of edges
// Space Complexity: O(V)
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        int n = graph.length;
        int[] color = new int[n];
        List<Integer> result = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            if (dfs(i, graph, color)) {
                result.add(i);
            }
        }

        return result;
    }

    public boolean dfs(int node, int[][] graph, int[] color) {
        if (color[node] > 0) {
            return color[node] == 2;
        }

        color[node] = 1;
        for (int neighbor : graph[node]) {
            if (color[neighbor] == 2) {
                continue;
            }
            if (color[neighbor] == 1 || !dfs(neighbor, graph, color)) {
                return false;
            }
        }
        color[node] = 2;
        return true;
    }
}
```

### Solution
go
```go
// Time Complexity: O(V + E), where V is the number of vertices and E is the number of edges
// Space Complexity: O(V)
import "sort"

func eventualSafeNodes(graph [][]int) []int {
    n := len(graph)
    color := make([]int, n)
    var result []int

    for i := 0; i < n; i++ {
        if dfs(i, graph, color) {
            result = append(result, i)
        }
    }

    sort.Ints(result) // Sort the result to maintain order
    return result
}

func dfs(node int, graph [][]int, color []int) bool {
    if color[node] > 0 {
        return color[node] == 2
    }

    color[node] = 1
    for _, neighbor := range graph[node] {
        if color[neighbor] == 2 {
            continue
        }
        if color[neighbor] == 1 || !dfs(neighbor, graph, color) {
            return false
        }
    }
    color[node] = 2
    return true
}
```

