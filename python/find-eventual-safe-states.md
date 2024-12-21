# [Leetcode 802: Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)

## Approaches

1. [Topological Sort using Indegree](#approach-1-topological-sort-using-indegree)
2. [DFS with Cycle Detection](#approach-2-dfs-with-cycle-detection)
3. [Iterative DFS with Explicit Stack](#approach-3-iterative-dfs-with-explicit-stack)

---

## Approach 1: Topological Sort using Indegree

**Intuition**: The basic idea is to consider the graph in reverse (i.e., reverse the direction of each edge). When looking for terminal nodes (safe states), we need to examine nodes whose indegree is zero (i.e., nodes that have no other nodes leading into them in a reversed graph). These nodes can be considered starting points in the original graph because they have no outgoing edges.

**Solution**:
- Reverse the graph.
- Compute in-degrees for each node in the reversed graph.
- Perform a topological sort and identify all nodes that can eventually reach terminal nodes.

```python
def eventualSafeNodes(graph):
    from collections import deque, defaultdict

    n = len(graph)

    # Reverse the graph
    reversed_graph = defaultdict(list)
    indegree = [0] * n

    for start_node, end_nodes in enumerate(graph):
        for end_node in end_nodes:
            reversed_graph[end_node].append(start_node)
            indegree[start_node] += 1

    queue = deque([node for node in range(n) if indegree[node] == 0])
    safe_nodes = []

    while queue:
        node = queue.popleft()
        safe_nodes.append(node)
        
        for neighbor in reversed_graph[node]:
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                queue.append(neighbor)

    return sorted(safe_nodes)

# Time Complexity: O(V + E), where V is the number of vertices and E is the number of edges.
# Space Complexity: O(V + E), for storing the reversed graph.
```

---

## Approach 2: DFS with Cycle Detection

**Intuition**: A node is considered "safe" if there is no path beginning at that node and ending in a cycle. We can use Depth-first search (DFS) to detect cycles and classify nodes.

**Solution**:
- Use three states for nodes: `0` for unvisited, `1` for visiting, and `2` for safe.
- Perform DFS, marking nodes as "visiting" when starting, and "safe" if completely processed with no cycles detected.

```python
def eventualSafeNodes(graph):
    n = len(graph)
    state = [0] * n

    def dfs(node):
        if state[node] > 0:
            return state[node] == 2
        state[node] = 1
        for neighbor in graph[node]:
            if state[neighbor] == 1 or not dfs(neighbor):
                return False
        state[node] = 2
        return True

    return [node for node in range(n) if dfs(node)]

# Time Complexity: O(V + E), where V is the number of vertices and E is the number of edges.
# Space Complexity: O(V), for the state array.
```

---

## Approach 3: Iterative DFS with Explicit Stack

**Intuition**: Similar to the DFS approach with cycle detection, but instead of using recursion, use an explicit stack to manage the DFS process. This can be useful to avoid recursion limit issues and manage state explicitly for each node processed.

**Solution**:
- Use an explicit stack to simulate the DFS process.
- Keep track of node states and manage transitions between "visiting" and "safe".

```python
def eventualSafeNodes(graph):
    n = len(graph)
    state = [0] * n
    stack = []

    def iterative_dfs(start):
        stack.append(start)
        stack_state = {start: 1}

        while stack:
            node = stack[-1]
            if stack_state[node] == 2:
                if state[node] == 0:
                    state[node] = 2
                stack.pop()
                continue

            if state[node] == 0:
                state[node] = 1

            for neighbor in graph[node]:
                if state[neighbor] == 1 or (neighbor in stack_state and stack_state[neighbor] == 1):
                    return False
                if state[neighbor] == 0:
                    stack.append(neighbor)
                    stack_state[neighbor] = 1

            stack_state[node] = 2

        return True

    return [node for node in range(n) if iterative_dfs(node)]

# Time Complexity: O(V + E), where V is the number of vertices and E is the number of edges.
# Space Complexity: O(V), for the state and stack structures.
```

