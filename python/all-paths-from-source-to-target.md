# [797. All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approaches
1. [DFS with Backtracking](#approach-1-dfs-with-backtracking)
2. [BFS with Queue](#approach-2-bfs-with-queue)

---

## Approach 1: DFS with Backtracking

### Intuition
The problem involves finding all possible paths from a source node to a target node in a Directed Acyclic Graph (DAG). A natural way to explore all paths is by using Depth First Search (DFS). Starting from the source node, we attempt to reach the target by recursively visiting each adjacent node. We use backtracking to ensure that we explore all paths.

### Steps
1. Start DFS from the source node (node 0).
2. Keep track of the current path.
3. If the destination node is reached, add the current path to the results.
4. If not, explore each neighbor of the current node recursively.
5. Use backtracking to remove the current node from the path before returning.

### Time Complexity
- **O(2^V \cdot V)**, where V is the number of vertices. In the worst case, for each node, you explore two recursive branches.

### Space Complexity
- **O(V)** for recursive stack (excluding output space).

```python
def allPathsSourceTarget(graph):
    def dfs(node, path):
        # If target node is reached, add the path to answer
        if node == len(graph) - 1:
            result.append(list(path))
            return
        # Explore neighbors
        for neighbor in graph[node]:
            path.append(neighbor)
            dfs(neighbor, path)
            path.pop()  # Backtrack

    result = []
    dfs(0, [0])
    return result

# Example usage:
# graph = [[1,2],[3],[3],[]]
# print(allPathsSourceTarget(graph))  # Output: [[0,1,3],[0,2,3]]
```

---

## Approach 2: BFS with Queue

### Intuition
An alternative approach is using Breadth First Search (BFS). Instead of diving deep into a path like DFS, BFS explores all possible paths step-by-step layer. By using a queue, we build paths incrementally and check if the destination node has been reached at each step.

### Steps
1. Initialize a queue with a path starting from the source node.
2. While the queue is not empty, extract the path from the queue.
3. Check if the last node of the path is the target, add the path to results.
4. Otherwise, enqueue new paths extending the current path by each neighbor of the last node.

### Time Complexity
- **O(2^V \cdot V)**, where V is the number of vertices.

### Space Complexity
- **O(V \cdot 2^V)** due to storage of paths.

```python
from collections import deque

def allPathsSourceTarget(graph):
    target = len(graph) - 1
    queue = deque([[0]])  # Starting queue with path from source
    result = []

    while queue:
        path = queue.popleft()

        # Last node in the current path
        node = path[-1]

        if node == target:
            result.append(path)
        else:
            for neighbor in graph[node]:
                new_path = list(path)
                new_path.append(neighbor)
                queue.append(new_path)

    return result

# Example usage:
# graph = [[1,2],[3],[3],[]]
# print(allPathsSourceTarget(graph))  # Output: [[0,1,3],[0,2,3]]
```

---
These two methods demonstrate the diverse ways of traversing a graph to solve the paths finding problem, each highlighting DFS and BFS's strengths and weaknesses.

