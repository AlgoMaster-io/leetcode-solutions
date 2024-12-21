# [Leetcode 1203: Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)

## Table of Contents
- [Approach 1: Kahn's Algorithm for Topological Sorting with Separate Group and Item Graphs](#approach-1-kahns-algorithm-for-topological-sorting-with-separate-group-and-item-graphs)

## Approach 1: Kahn's Algorithm for Topological Sorting with Separate Group and Item Graphs

### Intuition
This problem asks us to sort items such that items are sorted respecting the group they belong to and their dependencies. Here's how we can tackle the problem:

1. **Understanding the Problem**: We have items divided into groups. We need to sort the items such that for each group, items and their dependencies are sorted. Besides, items between different groups must be sorted based on inter-group dependencies. This demands a topological sort.

2. **Topological Sort**: Ordinarily, a topological sort is used when we need to order tasks based on dependencies (directed acyclic graph - DAG). If node A depends on node B, then B should appear before A in the ordering. Here, we have two levels of ordering:  
   - Within each group
   - Among groups

3. **Graph Representation**: We will use two graphs:
   - **Group Graph**: To handle dependencies between different groups.
   - **Item Graph**: To handle dependencies within the same group.

4. **Kahn's Algorithm**: It is a typical algorithm for finding topological ordering of nodes in a directed graph. We will use Kahn’s algorithm to perform the topological sorting on both graphs.

5. **Steps**: 
   - Assign items to a designated group if they are not part of a group (-1 to a unique number).
   - Build item-level and group-level graphs, keeping track of in-degrees.
   - Perform topological sorting on these graphs.
   - Use the result of group topological sorting to assemble the final item order.

### Time Complexity
- Time complexity is O(V + E) where V is the number of vertices (items and groups) and E is the number of edges (dependencies).

### Space Complexity
- Space complexity is O(V + E), space required for storing graphs and in-degrees.

### Solution

```typescript
function sortItems(n: number, m: number, group: number[], beforeItems: number[][]): number[] {
    // If a group is unassigned (-1), assign a unique ID starting from m (to avoid conflict with existing groups)
    for (let i = 0; i < n; i++) {
        if (group[i] === -1) {
            group[i] = m;
            m++;
        }
    }
    
    // Create adjacency lists and in-degree counters
    const itemGraph: Map<number, number[]> = new Map();
    const itemInDegree: number[] = new Array(n).fill(0);
    const groupGraph: Map<number, number[]> = new Map();
    const groupInDegree: Map<number, number> = new Map<number, number>();

    for (let i = 0; i < n; i++) {
        itemGraph.set(i, []);
    }
    
    for (let i = 0; i < m; i++) {
        groupGraph.set(i, []);
        groupInDegree.set(i, 0);
    }
    
    // Fill the graphs
    for (let i = 0; i < n; i++) {
        let currentGroup = group[i];
        
        for (let beforeItem of beforeItems[i]) {
            let beforeGroup = group[beforeItem];
            
            if (currentGroup !== beforeGroup) {
                // Update group graph if there's an inter-group dependency
                if (!(groupGraph.get(beforeGroup) as number[]).includes(currentGroup)) {
                    (groupGraph.get(beforeGroup) as number[]).push(currentGroup);
                    groupInDegree.set(currentGroup, (groupInDegree.get(currentGroup) as number) + 1);
                }
            }
            
            // Update item graph
            (itemGraph.get(beforeItem) as number[]).push(i);
            itemInDegree[i]++;
        }
    }
    
    // Function to perform topological sort
    const topologicalSort = (nodes: number[], graph: Map<number, number[]>, inDegree: number[] | Map<number, number>): number[] | null => {
        let sortedOrder: number[] = [];
        let zeroInDegreeQueue: number[] = [];
        
        // Collect nodes with zero in-degree
        nodes.forEach(node => {
            if (inDegree[node] === 0) {
                zeroInDegreeQueue.push(node);
            }
        });
        
        // Process nodes with zero in-degree
        while (zeroInDegreeQueue.length > 0) {
            const current = zeroInDegreeQueue.shift() as number;  // dequeue the front
            sortedOrder.push(current);
            
            let neighbors = graph.get(current) || [];
            for (let neighbor of neighbors) {
                inDegree[neighbor]--;
                if (inDegree[neighbor] === 0) {
                    zeroInDegreeQueue.push(neighbor);
                }
            }
        }
        
        // Check if sorting was possible (i.e., graph had no cycles)
        if (sortedOrder.length !== nodes.length) {
            return null;
        }
        return sortedOrder;
    }
    
    // Generate list of individual items and groups for sorting function
    const itemList: number[] = Array.from(Array(n).keys());
    const groupList: number[] = Array.from(groupInDegree.keys());
    
    // Topological sort groups and items
    const sortedGroups = topologicalSort(groupList, groupGraph, Array.from(groupInDegree.values()) as number[]);
    const sortedItems = topologicalSort(itemList, itemGraph, itemInDegree);

    if (!sortedGroups || !sortedItems) return [];
    
    // Map group to its corresponding items
    const groupToItems = new Map<number, number[]>();
    for (let item of sortedItems) {
        if (!groupToItems.has(group[item])) groupToItems.set(group[item], []);
        (groupToItems.get(group[item]) as number[]).push(item);
    }
    
    // Assemble the result, respecting group ordering
    let result: number[] = [];
    for (let g of sortedGroups) {
        result.push(...(groupToItems.get(g) || []));
    }
    
    return result;
}
```

This solution efficiently constructs both the group-level and item-level dependency graphs and performs topological sorting to determine the correct order for processing items, taking into account both intra and inter-group dependencies.

