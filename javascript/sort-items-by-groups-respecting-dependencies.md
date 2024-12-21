# [Leetcode 1203: Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)

## Approaches
1. [Topological Sort with Two Graphs](#approach-1---topological-sort-with-two-graphs)

## Approach 1 - Topological Sort with Two Graphs

### Intuition
To solve the problem of sorting items by groups while respecting dependencies, we can utilize topological sorting. The challenge is that we have both group dependencies and item dependencies. Hence, an effective approach is to use two separate graphs: one for groups and one for items.

1. **Graph for Groups**: This graph will represent dependencies between different groups. If a group `A` should come before another group `B`, we'll have a directed edge from `A` to `B`.

2. **Graph for Items**: Within each group, items will have dependencies expressed in this graph. An item `x` in the group should precede an item `y` if there exists an edge `x -> y`.

### Steps
1. **Build Graphs**:
   - Create adjacency lists and indegree counts for both groups and items.
   - For each item, if it belongs to a group, and its dependence lies in a different group, establish an edge between these groups.
   - Also, establish an edge between items according to the dependencies list.

2. **Topological Sort**:
   - Perform a topological sort first on the group graph. If it’s impossible, return an empty list since the overall order is not feasible.
   - For each group, perform a topological sort on the items within that group.

3. **Compile Results**:
   - With the sorted list of groups, order the items as per the topological order found and concatenate results for a final sorted list.

### Time Complexity
- Building the graphs costs `O(n + m)`, where `n` is the number of items and `m` is the sum of dependencies.
- Topological sort also costs `O(n + m)`.
- Overall, the complexity is `O(n + m)`.

### Space Complexity
- Space used is `O(n + m)` for storing the graphs and indegrees.

### Code
```javascript
var sortItems = function(n, m, group, beforeItems) {
    // Initialize items-to-group if uncategorized
    for (let i = 0; i < n; i++) {
        if (group[i] === -1) {
            group[i] = m;
            m++;
        }
    }
    
    // Adjacency lists and indegree arrays
    const groupAdjList = Array.from({ length: m }, () => []);
    const itemAdjList = Array.from({ length: n }, () => []);
    const groupIndegree = Array(m).fill(0);
    const itemIndegree = Array(n).fill(0);
    
    // Build graphs
    for (let i = 0; i < n; i++) {
        for (const before of beforeItems[i]) {
            itemAdjList[before].push(i);
            itemIndegree[i]++;
            if (group[i] !== group[before]) {
                groupAdjList[group[before]].push(group[i]);
                groupIndegree[group[i]]++;
            }
        }
    }
    
    // Topological Sort Function
    const topologicalSort = (adjList, indegree, totalItems) => {
        const queue = [];
        for (let i = 0; i < totalItems; i++) {
            if (indegree[i] === 0) queue.push(i);
        }
        
        const sortedArray = [];
        while (queue.length) {
            const node = queue.shift();
            sortedArray.push(node);
            for (const neighbor of adjList[node]) {
                indegree[neighbor]--;
                if (indegree[neighbor] === 0) queue.push(neighbor);
            }
        }
        
        return sortedArray.length === totalItems ? sortedArray : [];
    };
    
    // Sort groups and detect cycle
    const groupOrder = topologicalSort(groupAdjList, groupIndegree, m);
    if (groupOrder.length === 0) return [];
    
    // Sort items within each group
    const itemOrder = topologicalSort(itemAdjList, itemIndegree, n);
    if (itemOrder.length === 0) return [];
    
    // Group items by group order
    const itemsInGroup = Array.from({ length: m }, () => []);
    for (const item of itemOrder) {
        itemsInGroup[group[item]].push(item);
    }
    
    // Compile sorted items according to sorted groups
    const result = [];
    for (const grp of groupOrder) {
        result.push(...itemsInGroup[grp]);
    }
    
    return result;
};
```

This solution effectively combines the constraints of both item and group dependencies using topological sorting, allowing for accurate handling of the problem requirements.

