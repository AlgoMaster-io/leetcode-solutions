# [1203. Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)

## Approaches:
1. [Basic Topological Sort with Two Separate Graphs](#approach-1)
2. [Advanced Topological Sort with Single Graph](#approach-2)

---

## Approach 1: Basic Topological Sort with Two Separate Graphs <a name="approach-1"></a>

### Intuition
The problem can be broken down into two separate topological sort problems: one for the items and another for the groups. Each item belongs to a group, and items and groups must be reordered to respect their respective dependencies.

The plan is to:
1. Separate items into groups and form two graphs: one for item dependencies and one for group dependencies.
2. Use topological sorting to sort items within a group and then sort groups themselves.

### Steps
1. We will first set up the item graph and group graph using adjacency lists.
2. Perform topological sort on the items within each group.
3. Perform topological sort on the groups themselves.

### Time Complexity
- Building the graphs takes O(n + m), where n is the number of items and m is the number of relations.
- Topological sort for each graph takes O(n + m) as well.
- Overall complexity is O(n + m).

### Space Complexity
- Storing the graphs requires O(n + m) space.

### Java Code
```java
import java.util.*;

public class Solution {

    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
        
        // Initialize the group ids for standalone items.
        for (int i = 0; i < n; i++) {
            if (group[i] == -1) {
                group[i] = m++;
            }
        }
        
        // Create adjacency lists for items and groups.
        List<Integer>[] itemGraph = new List[n];
        List<Integer>[] groupGraph = new List[m];
        for (int i = 0; i < n; i++) {
            itemGraph[i] = new ArrayList<>();
        }
        for (int i = 0; i < m; i++) {
            groupGraph[i] = new ArrayList<>();
        }
        
        // Indegree arrays for topological sorting.
        int[] itemIndegree = new int[n];
        int[] groupIndegree = new int[m];
        
        // Populate the graphs.
        for (int i = 0; i < n; i++) {
            int currGroup = group[i];
            for (int beforeItem : beforeItems.get(i)) {
                // Add edge in the item graph.
                itemGraph[beforeItem].add(i);
                itemIndegree[i]++;
                
                int beforeGroup = group[beforeItem];
                if (beforeGroup != currGroup) {
                    // Add edge in the group graph if in different groups.
                    groupGraph[beforeGroup].add(currGroup);
                    groupIndegree[currGroup]++;
                }
            }
        }
        
        // Topological sort for the groups.
        List<Integer> sortedGroups = topologicalSort(groupGraph, groupIndegree, m);
        if (sortedGroups == null) return new int[0];
        
        // Topological sort for individuals in each group.
        List<Integer> result = new ArrayList<>();
        for (int grp : sortedGroups) {
            List<Integer> itemsInGroup = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                if (group[i] == grp) {
                    itemsInGroup.add(i);
                }
            }
            List<Integer> sortedItems = topologicalSort(itemGraph, itemIndegree, itemsInGroup.size(), itemsInGroup);
            if (sortedItems == null) return new int[0];
            result.addAll(sortedItems);
        }
        
        return result.stream().mapToInt(i -> i).toArray();
    }
    
    private List<Integer> topologicalSort(List<Integer>[] graph, int[] indegree, int total, List<Integer> nodes) {
        Queue<Integer> queue = new LinkedList<>();
        for (int node : nodes) {
            if (indegree[node] == 0) {
                queue.add(node);
            }
        }
        
        List<Integer> sorted = new ArrayList<>();
        while (!queue.isEmpty()) {
            int node = queue.poll();
            sorted.add(node);
            for (int neighbor : graph[node]) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) {
                    queue.add(neighbor);
                }
            }
        }
        
        return sorted.size() == total ? sorted : null;
    }
    
    private List<Integer> topologicalSort(List<Integer>[] graph, int[] indegree, int size) {
        return topologicalSort(graph, indegree, size, generateRange(size));
    }
    
    private List<Integer> generateRange(int n) {
        List<Integer> range = new ArrayList<>(n);
        for (int i = 0; i < n; i++) {
            range.add(i);
        }
        return range;
    }
}
```

---

## Approach 2: Advanced Topological Sort with Single Graph <a name="approach-2"></a>

### Intuition
To optimize further, we combine the management of items and groups into a single graph. We treat groups as virtual nodes and manage dependencies among them inline with item dependencies. This removes the dependency on two separate graph traversals.

### Steps
1. Items are assigned to their respective groups or created as standalone groups for unassigned items.
2. A single graph tracks all group and item dependencies.
3. Perform a single topological sort to create a valid order of groups and items interspersed.

### Time Complexity
- O(n + m) due to the graph setup and traversal.

### Space Complexity
- O(n + m) for storing the graph.

### Java Code
```java
import java.util.*;

public class AdvancedSolution {

    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
        
        for (int i = 0; i < n; i++) {
            if (group[i] == -1) {
                group[i] = m++;
            }
        }
        
        List<Integer>[] combinedGraph = new List[n + m];
        for (int i = 0; i < n + m; i++) {
            combinedGraph[i] = new ArrayList<>();
        }
        
        int[] indegree = new int[n + m];
        
        for (int i = 0; i < n; i++) {
            int itemGroup = group[i];
            for (int beforeItem : beforeItems.get(i)) {
                int beforeItemGroup = group[beforeItem];
                if (beforeItemGroup == itemGroup) {
                    combinedGraph[beforeItem].add(i);
                    indegree[i]++;
                } else {
                    combinedGraph[beforeItemGroup + n].add(itemGroup + n);
                    indegree[itemGroup + n]++;
                }
            }
            combinedGraph[itemGroup + n].add(i);
            indegree[i]++;
        }
        
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n + m; i++) {
            if (indegree[i] == 0) {
                queue.add(i);
            }
        }

        List<Integer> result = new ArrayList<>();
        
        while (!queue.isEmpty()) {
            int node = queue.poll();
            if (node < n) {
                result.add(node);
            }
            for (int neighbor : combinedGraph[node]) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) {
                    queue.add(neighbor);
                }
            }
        }
        
        return result.size() == n ? result.stream().mapToInt(i -> i).toArray() : new int[0];
    }
}
```

In this approach, by expanding the graph to include groups as nodes as well, we can consolidate into a single topological sort pass. This is crucial for performance in large constraint scenarios, ensuring that we reduce the overhead seen with multiple passes and adjacency lists.

