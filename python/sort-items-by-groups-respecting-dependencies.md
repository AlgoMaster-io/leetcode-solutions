# [Leetcode 1203: Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)

## Approaches:
- [Approach 1: Topological Sort using Kahn's Algorithm](#approach-1-topological-sort-using-kahns-algorithm)
- [Approach 2: Topological Sort using Depth-First Search (DFS)](#approach-2-topological-sort-using-depth-first-search-dfs)

## Approach 1: Topological Sort using Kahn's Algorithm

### Intuition:
In this problem, we need to perform a topological sorting not only for individual items based on their dependency but also for groups. We need to ensure that all items of a group are handled before the items of its dependent group. The problem suggests a topological sorting problem on both items and groups.

1. **Subgraph Construction:** 
   - First, separate items into groups. For an item not assigned to any group, consider it as belonging to a unique group.
   - Construct the dependency graph for both items and groups. 
   - Ensure that items respect their dependency constraints within their own group as well as between groups.

2. **Topological Sorting for Items and Groups:**
   - Perform a topological sort on these graphs using a variation of Kahn's algorithm which handles dependencies through counting incoming edges.

### Steps:
1. **Group assignments:** Reassign un-grouped items to their own unique groups.
2. **Graphs Creation**:
   - Create two dependency graphs: One for items and another for groups.
3. **Topological Sorting:**
   - Utilize Kahn's algorithm to perform topological sort on both the item dependency graph and group dependency graph.
   - Ensure items are sorted according to their group topological order.

### Code:
```python
from collections import defaultdict, deque

def sortItems(n, m, group, beforeItems):
    # Assign a unique group ID to ungrouped items
    for i in range(n):
        if group[i] == -1:
            group[i] = m
            m += 1
    
    # Build dependency graph
    item_graph = defaultdict(list)
    item_indegree = [0] * n
    group_graph = defaultdict(list)
    group_indegree = [0] * m
    
    for i in range(n):
        for pre in beforeItems[i]:
            item_graph[pre].append(i)
            item_indegree[i] += 1
            # Handle group level dependencies
            if group[pre] != group[i]:
                group_graph[group[pre]].append(group[i])
                group_indegree[group[i]] += 1

    # Function to perform topological sort
    def topologicalSort(graph, indegree, size):
        queue = deque([node for node in range(size) if indegree[node] == 0])
        result = []
        
        while queue:
            node = queue.popleft()
            result.append(node)
            for neighbor in graph[node]:
                indegree[neighbor] -= 1
                if indegree[neighbor] == 0:
                    queue.append(neighbor)
        
        if len(result) == size:
            return result
        else:
            return []  # Cycle detected

    # Topological sort for groups
    group_order = topologicalSort(group_graph, group_indegree, m)
    if len(group_order) == 0:
        return []  # Cycle detected in group level
    
    # Topological sort for items
    item_order = topologicalSort(item_graph, item_indegree, n)
    if len(item_order) == 0:
        return []  # Cycle detected in item level
    
    # Organize items according to group order
    group_to_items = defaultdict(list)
    for item in item_order:
        group_to_items[group[item]].append(item)
    
    sorted_items = []
    for grp in group_order:
        sorted_items.extend(group_to_items[grp])
    
    return sorted_items
```

### Time Complexity:
- Constructing the graph and topological sort: O(n + m + e + g),
  - Where `n` is the number of items,
  - `m` is the number of groups,
  - `e` is the number of dependencies,
  - `g` is the number of group-level dependencies.

### Space Complexity:
- O(n + m) for storing the graphs and indegree counts.

## Approach 2: Topological Sort using Depth-First Search (DFS)

### Intuition:
The DFS-based topological sort offers an alternative approach by recursively traversing nodes. It detects cycles and ensures the ordering of nodes directly through the call stack. Here, the inherent recursive nature better handles intricate dependencies encountered in multi-level dependency graphs.

### Steps:
- Similar to Kahn's algorithm: Construct dependency graphs.
- Implement a DFS to recursively detect and manage dependency order through dynamic construction and examination of a traversal stack.

### Code:
```python
def sortItemsUsingDFS(n, m, group, beforeItems):
    # Assign a unique group ID to ungrouped items
    for i in range(n):
        if group[i] == -1:
            group[i] = m
            m += 1

    item_graph = defaultdict(list)
    group_graph = defaultdict(list)

    for i in range(n):
        for pre in beforeItems[i]:
            item_graph[pre].append(i)
            if group[pre] != group[i]:
                group_graph[group[pre]].append(group[i])

    def dfs(graph, visited, node, stack):
        if visited[node] == -1:
            return False  # Cycle detected
        if visited[node] == 1:
            return True  # Already visited
        
        visited[node] = -1  # Mark as part of recursion stack
        for neighbor in graph[node]:
            if not dfs(graph, visited, neighbor, stack):
                return False
        visited[node] = 1  # Mark as fully processed
        stack.append(node)
        return True

    # function to do topological sort using DFS
    def topologicalSortDFS(graph, size):
        visited = [0] * size
        stack = []
        
        for node in range(size):
            if visited[node] == 0:
                if not dfs(graph, visited, node, stack):
                    return []  # Cycle detected
        return stack[::-1]  # Return in correct order

    group_order = topologicalSortDFS(group_graph, m)
    if not group_order:
        return []  # Cycle detected

    item_order = topologicalSortDFS(item_graph, n)
    if not item_order:
        return []  # Cycle detected

    group_to_items = defaultdict(list)
    for item in item_order:
        group_to_items[group[item]].append(item)

    sorted_items = []
    for grp in group_order:
        sorted_items.extend(group_to_items[grp])

    return sorted_items
```

### Time Complexity:
- O(n + m + e + g), similar to Kahn's, as DFS requires visiting all nodes and edges to ensure no cycles.

### Space Complexity:
- O(n + m) for stack storage during DFS and graph.

