# [1203. Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)

## Approaches
- [Approach 1: Topological Sort with DFS](#approach-1-topological-sort-with-dfs)
- [Approach 2: Topological Sort with Kahn's Algorithm](#approach-2-topological-sort-with-kahns-algorithm)

## Approach 1: Topological Sort with DFS

The problem requires us to sort a collection of items subject to intra-group dependencies and inter-group dependencies. Here's a step-by-step breakdown:

### Intuition:
1. **Dependency Graph Construction**: 
    - Construct two parallel graphs: one for items within each group (intra-group) and another for groups based on inter-group dependencies.
   
2. **Topological Sort**:
    - Perform a topological sort on both graphs. The goal is to resolve dependencies without violating any before dependencies.

3. **DFS for Topological Sort**:
    - Use Depth-First Search (DFS) to detect cycles for cycle detection in each graph and determine the order.

### Steps:
- Divide items into groups, updating non-grouped items.
- Build the item dependency graph considering both intra-group and inter-group dependencies.
- Use DFS with backtracking for topological sorting to resolve dependency ordering.

### Code:
```go
func topologicalSortDFS(graph map[int][]int, inDegree []int, n int) ([]int, bool) {
	stack := []int{}
	result := []int{}

	// Push all nodes with in-degree 0.
	for i := 0; i < n; i++ {
		if inDegree[i] == 0 {
			stack = append(stack, i)
		}
	}

	for len(stack) > 0 {
		node := stack[len(stack)-1]
		stack = stack[:len(stack)-1]
		result = append(result, node)

		for _, nei := range graph[node] {
			inDegree[nei]--
			if inDegree[nei] == 0 {
				stack = append(stack, nei)
			}
		}
	}

	// If the result does not contain all nodes, there is a cycle.
	return result, len(result) == n
}

func sortItems(n int, m int, group []int, beforeItems [][]int) []int {
	// Assign IDs to ungrouped items.
	nextGroupID := m
	for i := 0; i < n; i++ {
		if group[i] == -1 {
			group[i] = nextGroupID
			nextGroupID++
		}
	}

	// Construct intra group graph for items, and group graph.
	itemGraph := make(map[int][]int)
	groupGraph := make(map[int][]int)
	itemInDeg := make([]int, n)
	groupInDeg := make([]int, nextGroupID)

	// Populate graphs.
	for cur, deps := range beforeItems {
		curGroup := group[cur]
		for _, pre := range deps {
			preGroup := group[pre]
			if curGroup == preGroup {
				// Intra-group dependencies
				itemGraph[pre] = append(itemGraph[pre], cur)
				itemInDeg[cur]++
			} else {
				// Inter-group dependencies
				groupGraph[preGroup] = append(groupGraph[preGroup], curGroup)
				groupInDeg[curGroup]++
			}
		}
	}

	// Topo sort for items and groups.
	sortedItems, validItems := topologicalSortDFS(itemGraph, itemInDeg, n)
	sortedGroups, validGroups := topologicalSortDFS(groupGraph, groupInDeg, nextGroupID)

	if !validItems || !validGroups {
		return []int{} // Cycle or unresolved dependency
	}

	// Map group items.
	groupToItems := make(map[int][]int)
	for _, item := range sortedItems {
		groupID := group[item]
		groupToItems[groupID] = append(groupToItems[groupID], item)
	}

	// Create final sorted list based on group order.
	result := []int{}
	for _, grp := range sortedGroups {
		result = append(result, groupToItems[grp]...)
	}

	return result
}
```

### Time and Space Complexity:
- **Time Complexity**: \(O(V + E)\), where \(V\) is the number of items and groups, and \(E\) is the total number of dependencies.
- **Space Complexity**: \(O(V + E)\), needed for storing graphs and in-degrees.

## Approach 2: Topological Sort with Kahn's Algorithm

In this approach, we leverage Kahn's algorithm to perform a topological sort which uses a queue and is typically more intuitive than DFS-based approaches.

### Intuition:
1. **Kahn's Algorithm**:
   - It iterates through nodes of a graph starting with nodes having zero in-degree.
   - This can naturally accommodate dynamic structures like intermittent dependencies.

2. **Implementation**:
   - Construct item and group dependency graphs.
   - Use Kahn’s algorithm to resolve the order.

### Code:
```go
func topologicalSortKahn(graph map[int][]int, inDegree []int, n int) ([]int, bool) {
	queue := []int{}
	result := []int{}

	// Populate initial queue with zero in-degree nodes
	for i := 0; i < n; i++ {
		if inDegree[i] == 0 {
			queue = append(queue, i)
		}
	}

	for len(queue) > 0 {
		node := queue[0]
		queue = queue[1:]
		result = append(result, node)

		for _, nei := range graph[node] {
			inDegree[nei]--
			if inDegree[nei] == 0 {
				queue = append(queue, nei)
			}
		}
	}

	// Check for cycle
	return result, len(result) == n
}

func sortItemsWithKahn(n int, m int, group []int, beforeItems [][]int) []int {
	nextGroupID := m
	for i := 0; i < n; i++ {
		if group[i] == -1 {
			group[i] = nextGroupID
			nextGroupID++
		}
	}

	itemGraph := make(map[int][]int)
	groupGraph := make(map[int][]int)
	itemInDeg := make([]int, n)
	groupInDeg := make([]int, nextGroupID)

	for cur, deps := range beforeItems {
		curGroup := group[cur]
		for _, pre := range deps {
			preGroup := group[pre]
			if curGroup == preGroup {
				itemGraph[pre] = append(itemGraph[pre], cur)
				itemInDeg[cur]++
			} else {
				groupGraph[preGroup] = append(groupGraph[preGroup], curGroup)
				groupInDeg[curGroup]++
			}
		}
	}

	sortedItems, validItems := topologicalSortKahn(itemGraph, itemInDeg, n)
	sortedGroups, validGroups := topologicalSortKahn(groupGraph, groupInDeg, nextGroupID)

	if !validItems || !validGroups {
		return []int{}
	}

	groupToItems := make(map[int][]int)
	for _, item := range sortedItems {
		groupID := group[item]
		groupToItems[groupID] = append(groupToItems[groupID], item)
	}

	result := []int{}
	for _, grp := range sortedGroups {
		result = append(result, groupToItems[grp]...)
	}

	return result
}
```

### Time and Space Complexity:
- **Time Complexity**: \(O(V + E)\), as the algorithm goes through each node and edge once.
- **Space Complexity**: \(O(V + E)\), similar to Approach 1, storing graphs and in-degrees for nodes and groups.

Each of these approaches provides a pathway to solve the complex dependency graph sorting issue efficiently, with both utilizing fundamental topological sorting techniques.

