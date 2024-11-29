# 797. [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approach: Depth-First Search (DFS)

### Solution
```python
# Time Complexity: O(2^n * n), where n is the number of nodes in the graph
# Space Complexity: O(2^n * n), for storing all paths in the result

class Solution:
    def allPathsSourceTarget(self, graph):
        def dfs(node, path):
            if node == len(graph) - 1:
                # If we reach the target node, add the current path to the result
                result.append(list(path))
                return
            
            for neighbor in graph[node]:
                path.append(neighbor)  # Add the neighbor to the current path
                dfs(neighbor, path)    # Recur for the neighbor
                path.pop()             # Backtrack to explore other paths

        result = []
        path = [0]  # Start from the source node
        dfs(0, path)
        return result
```

