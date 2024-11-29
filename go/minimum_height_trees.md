# 310. [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

## Approach 1: Using Breadth-First Search (BFS) to Find Leaf Nodes

### Solution

java
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.ArrayList;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if (n == 1) return List.of(0); // Base case for single node

        List<HashSet<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new HashSet<>());
        }

        // Build the graph
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }

        // Initialize leaves queue
        Queue<Integer> leaves = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (graph.get(i).size() == 1) {
                leaves.add(i);
            }
        }

        // Trim leaves until reaching the core
        while (n > 2) {
            int leafCount = leaves.size();
            n -= leafCount; // Decrease count of nodes

            for (int i = 0; i < leafCount; i++) {
                int leaf = leaves.poll();
                for (int neighbor : graph.get(leaf)) {
                    graph.get(neighbor).remove(leaf);
                    if (graph.get(neighbor).size() == 1) {
                        leaves.offer(neighbor);
                    }
                }
            }
        }

        return new ArrayList<>(leaves);
    }
}
```

### Solution

go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
import (
    "container/list"
)

func findMinHeightTrees(n int, edges [][]int) []int {
    if n == 1 {
        return []int{0} // Base case for single node
    }

    // Build the graph using adjacency list
    graph := make([][]int, n)
    for _, edge := range edges {
        graph[edge[0]] = append(graph[edge[0]], edge[1])
        graph[edge[1]] = append(graph[edge[1]], edge[0])
    }

    // Initialize leaves queue
    leaves := list.New()
    for i := 0; i < n; i++ {
        if len(graph[i]) == 1 {
            leaves.PushBack(i)
        }
    }

    // Trim leaves until reaching the core
    for n > 2 {
        leafCount := leaves.Len()
        n -= leafCount

        for i := 0; i < leafCount; i++ {
            leaf := leaves.Remove(leaves.Front()).(int)
            for _, neighbor := range graph[leaf] {
                graph[neighbor] = removeElement(graph[neighbor], leaf)
                if len(graph[neighbor]) == 1 {
                    leaves.PushBack(neighbor)
                }
            }
        }
    }

    result := make([]int, 0, leaves.Len())
    for e := leaves.Front(); e != nil; e = e.Next() {
        result = append(result, e.Value.(int))
    }
    return result
}

func removeElement(slice []int, element int) []int {
    for i, v := range slice {
        if v == element {
            return append(slice[:i], slice[i+1:]...)
        }
    }
    return slice
}
```

