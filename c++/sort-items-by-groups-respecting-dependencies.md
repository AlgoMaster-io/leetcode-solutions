# [Leetcode 1203: Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)

## Approaches:
- [Approach 1: Topological Sorting with BFS](#approach-1-topological-sorting-with-bfs)
- [Approach 2: Topological Sorting with DFS](#approach-2-topological-sorting-with-dfs)

### Approach 1: Topological Sorting with BFS

#### Intuition:
The problem presents a complex structure involving both items and groups with dependencies that need to be sorted topologically. The intuition to solve this problem involves using two levels of topological sorting:
1. **Item-level Sorting**: Sort the items within each group and among items without a group.
2. **Group-level Sorting**: Sort the groups as if they were individual nodes.

By sorting the items and groups using topological sorting, we can address the dependencies issue.

The problem is tackled by:
- Creating an adjacency list for groups and items.
- Applying topological sort to ensure that all dependencies are respected.

#### Steps:
1. **Create an adjacency list of items and groups**: Build a graph where each node is an item or a group, and each edge represents a dependency.
2. **Topological Sort using BFS (Kahn's Algorithm)**: Leverage BFS to perform topological sorting on the graph built.
3. **Verify and Assemble**: After sorting, verify if a valid order can be formed considering all dependencies.

Here's the detailed implementation:

```cpp
#include <vector>
#include <queue>
#include <unordered_map>
#include <iostream>

using namespace std;

vector<int> topSort(int n, vector<int>& indegree, unordered_map<int, vector<int>>& adj) {
    queue<int> q; // Queue for managing the current processing layer of items
    vector<int> order; // Holds the final order of items or groups
    
    // Initial queue population with nodes having zero indegrees
    for (int i = 0; i < n; ++i) {
        if (indegree[i] == 0) {
            q.push(i);
        }
    }
    
    // Topological sort
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        // Add current node to the sorted order
        order.push_back(node);
        
        // Process neighbors of current node
        for (int neighbor : adj[node]) {
            if (--indegree[neighbor] == 0) {
                q.push(neighbor);
            }
        }
    }
    
    // Return empty if topological sorting isn't possible due to a cycle
    return (order.size() == n) ? order : vector<int>();
}

vector<int> sortItems(int n, int m, vector<int>& group, vector<vector<int>>& beforeItems) {
    int numGroups = m;
    
    // Assign unique groups to items without a defined group
    for (int i = 0; i < n; ++i) {
        if (group[i] == -1) {
            group[i] = numGroups++;
        }
    }
    
    // Adjacency lists and indegree arrays
    unordered_map<int, vector<int>> groupGraph, itemGraph;
    vector<int> groupIndegree(numGroups), itemIndegree(n);
    
    // Build the graphs and indegrees
    for (int i = 0; i < n; ++i) {
        for (int before : beforeItems[i]) {
            // Group dependencies
            if (group[i] != group[before]) {
                groupGraph[group[before]].push_back(group[i]);
                ++groupIndegree[group[i]];
            }
            
            // Item dependencies
            itemGraph[before].push_back(i);
            ++itemIndegree[i];
        }
    }
    
    // Sort groups and items
    vector<int> groupOrder = topSort(numGroups, groupIndegree, groupGraph);
    vector<int> itemOrder = topSort(n, itemIndegree, itemGraph);
    
    // Check feasibility
    if (groupOrder.empty() || itemOrder.empty()) {
        return {};
    }
    
    // Arrange items within their groups
    unordered_map<int, vector<int>> itemsInGroups;
    for (int item : itemOrder) {
        itemsInGroups[group[item]].push_back(item);
    }
    
    // Construct final sorted order
    vector<int> result;
    for (int grp : groupOrder) {
        for (int item : itemsInGroups[grp]) {
            result.push_back(item);
        }
    }
    
    return result;
}
```

#### Complexity Analysis:
- **Time Complexity**: `O(n + m)` - The time complexity for constructing graphs and performing topological sorting is linear with respect to the sum of the number of items and groups.
- **Space Complexity**: `O(n + m)` - Space is mainly utilized by adjacency lists and indegree arrays.

### Approach 2: Topological Sorting with DFS

#### Intuition:
This approach leverages depth-first search (DFS) for topological sorting. The idea is similar to the BFS approach, but instead of utilizing a queue, we employ recursive DFS to explore nodes. Once a node has been fully explored, it is pushed onto a stack. The stack, when fully constructed, will represent the topologically sorted order.

#### Steps:
1. **Graph Construction**: Similar to Approach 1, construct graphs for both the groups and items.
2. **DFS-based Topological Sort**: Employ DFS to determine the sorted order. Push nodes onto a stack after they are completely processed.
3. **Check Cycle**: Verify successful topological sorting by tracking cycle possibilities using visited states.

```cpp
#include <vector>
#include <unordered_map>
#include <iostream>

using namespace std;

bool dfsTopSort(int node, vector<int>& order, vector<int>& visited, unordered_map<int, vector<int>>& adj) {
    if (visited[node] == -1) return false; // Detect cycle while the node is in the current path
    if (visited[node] == 1) return true;   // Already visited node
    
    visited[node] = -1; // Mark node as visited in the current path
    for (int neighbor : adj[node]) {
        if (!dfsTopSort(neighbor, order, visited, adj)) return false;
    }
    visited[node] = 1; // Mark node as fully explored
    order.push_back(node); // Add node to the order
    
    return true;
}

vector<int> sortItems(int n, int m, vector<int>& group, vector<vector<int>>& beforeItems) {
    int numGroups = m;
    for (int i = 0; i < n; ++i) {
        if (group[i] == -1) {
            group[i] = numGroups++;
        }
    }
    
    unordered_map<int, vector<int>> groupGraph, itemGraph;
    vector<int> groupIndegree(numGroups), itemIndegree(n);
    
    for (int i = 0; i < n; ++i) {
        for (int before : beforeItems[i]) {
            if (group[i] != group[before]) {
                groupGraph[group[before]].push_back(group[i]);
                ++groupIndegree[group[i]];
            }
            itemGraph[before].push_back(i);
            ++itemIndegree[i];
        }
    }
    
    vector<int> groupOrder, itemOrder;
    vector<int> groupVisited(numGroups), itemVisited(n);
    
    for (int i = 0; i < numGroups; ++i) {
        if (groupVisited[i] == 0) {
            if (!dfsTopSort(i, groupOrder, groupVisited, groupGraph)) return {};
        }
    }
    
    for (int i = 0; i < n; ++i) {
        if (itemVisited[i] == 0) {
            if (!dfsTopSort(i, itemOrder, itemVisited, itemGraph)) return {};
        }
    }
    
    reverse(groupOrder.begin(), groupOrder.end());
    reverse(itemOrder.begin(), itemOrder.end());
    
    unordered_map<int, vector<int>> itemsInGroups;
    for (int item : itemOrder) {
        itemsInGroups[group[item]].push_back(item);
    }
    
    vector<int> result;
    for (int grp : groupOrder) {
        for (int item : itemsInGroups[grp]) {
            result.push_back(item);
        }
    }
    
    return result;
}
```

#### Complexity Analysis:
- **Time Complexity**: `O(n + m)` - Similar to BFS approach, the DFS traversal through all nodes and edges results in a linear time complexity.
- **Space Complexity**: `O(n + m)` - Space is needed for the adjacency lists, visited arrays, and the recursion stack in DFS. 

These approaches effectively handle topological sorting within the constraints of groups and dependencies using both BFS and DFS methods, ensuring compliance with the problem's requirements.

