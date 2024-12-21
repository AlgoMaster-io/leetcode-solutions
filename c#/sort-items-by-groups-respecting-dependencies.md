# [Leetcode Problem 1203: Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)

## Approaches
- [Approach 1: Topological Sort with Two-Level Graph Construction](#approach-1-topological-sort-with-two-level-graph-construction)

---

## Approach 1: Topological Sort with Two-Level Graph Construction

### Intuition:
The problem is a complex variant of topological sorting where we not only have dependencies among individual items but also have groups of items with their respective items' dependencies. The solution involves performing a two-level topological sort: one for the groups and another for the items within each group. Here’s the detailed approach:

1. **Separate Topological Sorts**: We first perform a topological sort for the groups. Each item belongs to a group, and hence, the group sorting respects the dependencies among items of different groups. Secondly, we perform a topological sort within each group to resolve dependencies amongst items in the same group.
  
2. **Handling Unassigned Groups**: If any item is not assigned to any group, assign it to its own unique group to simplify processing, treating them as isolated groups for sorting.
  
3. **Graph Construction**: Construct two graphs: one for item dependencies and another for group dependencies. This allows us to manage and sort dependencies on two levels independently.
  
4. **Cycle Detection**: Use a topological sort that includes cycle detection to ensure there is a valid order. If a cycle is detected in either graph, it implies no valid ordering exists.

5. **Combining the Results**: Once the topological order of items and groups is determined, combine the results respecting the two-level sort.

### Algorithm:

```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public int[] SortItems(int n, int m, int[] group, IList<IList<int>> beforeItems) {
        // Assign each item to its own unique group if it is unassigned
        for (int i = 0; i < n; i++) {
            if (group[i] == -1) {
                group[i] = m++;
            }
        }

        // Graph for items and indegree count
        List<int>[] itemGraph = new List<int>[n];
        int[] itemIndegree = new int[n];
        
        // Graph for groups and indegree count
        List<int>[] groupGraph = new List<int>[m];
        int[] groupIndegree = new int[m];

        for (int i = 0; i < n; i++) {
            itemGraph[i] = new List<int>();
        }
        
        for (int i = 0; i < m; i++) {
            groupGraph[i] = new List<int>();
        }
        
        for (int i = 0; i < n; i++) {
            foreach (int before in beforeItems[i]) {
                // Add edge in item graph
                itemGraph[before].Add(i);
                itemIndegree[i]++;
                
                // Add edge in group graph if they belong to different groups
                if (group[i] != group[before]) {
                    groupGraph[group[before]].Add(group[i]);
                    groupIndegree[group[i]]++;
                }
            }
        }
        
        // Get topological order for items and groups
        List<int> itemOrder = TopologicalSort(itemGraph, itemIndegree, n);
        List<int> groupOrder = TopologicalSort(groupGraph, groupIndegree, m);

        if (itemOrder.Count == 0 || groupOrder.Count == 0) {
            return new int[0]; // Cycle detected
        }

        // Map group order
        Dictionary<int, List<int>> groupedItems = new Dictionary<int, List<int>>();
        
        foreach (var item in itemOrder) {
            if (!groupedItems.ContainsKey(group[item])) {
                groupedItems[group[item]] = new List<int>();
            }
            groupedItems[group[item]].Add(item);
        }

        List<int> result = new List<int>();
        foreach (var grp in groupOrder) {
            if (groupedItems.ContainsKey(grp)) {
                result.AddRange(groupedItems[grp]);
            }
        }

        return result.ToArray();
    }

    private List<int> TopologicalSort(List<int>[] graph, int[] indegree, int totalNodes) {
        Queue<int> queue = new Queue<int>();
        List<int> result = new List<int>();
        
        // Initialize queue with nodes having indegree 0
        for (int i = 0; i < totalNodes; i++) {
            if (indegree[i] == 0) {
                queue.Enqueue(i);
            }
        }
        
        while (queue.Count > 0) {
            int node = queue.Dequeue();
            result.Add(node);
            
            foreach (var neighbor in graph[node]) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) {
                    queue.Enqueue(neighbor);
                }
            }
        }
        
        return result.Count == totalNodes ? result : new List<int>();
    }
}
```

### Time Complexity:
- Constructing the initial graphs takes O(E), where E is the number of dependencies.
- Performing two topological sorts (one for items, one for groups) each cost O(V + E), where V is the number of nodes, and E is the number of edges for that respective graph.
- The overall complexity is O(V + E) since each of n and m contribute separately, but n assembles the full runtime on items and m on groups, bounded by their separate dependencies.

### Space Complexity:
- O(V + E) for storing the graph structure and indegree arrays for the items and groups.

This approach efficiently handles dependencies both within and across groups, allowing for a clear establishment of execution order that satisfies all constraints.

