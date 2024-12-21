# [Leetcode 797: All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Approaches
- [DFS Backtracking](#dfs-backtracking)
- [BFS Using a Queue](#bfs-using-a-queue)

### DFS Backtracking

**Intuition:**
The problem is essentially asking for all paths from node `0` to node `n-1` in a Directed Acyclic Graph (DAG). DFS (Depth First Search) is well-suited for exploring all possibilities in the graph. By using a backtracking approach, we can explore each path from the source to the target and backtrack once we reach the end to explore alternative paths.

**Steps:**
1. Start DFS from node `0`. Explore each path by recursively visiting each node's neighbors.
2. Maintain a path list and whenever a node is visited, append it to the path.
3. If the current node is the target node `n-1`, add a copy of the path to the result list.
4. Backtrack by removing the node from the path list before exploring other neighbors.
5. Return all paths found once DFS exploration is complete.

```java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        List<List<Integer>> allPaths = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        path.add(0); // start from node 0
        dfs(graph, 0, path, allPaths);
        return allPaths;
    }
    
    private void dfs(int[][] graph, int currentNode, List<Integer> path, List<List<Integer>> allPaths) {
        // If current node is the last node in graph, add the path to results
        if (currentNode == graph.length - 1) {
            allPaths.add(new ArrayList<>(path));
            return;
        }
        // Explore each neighbor of the current node
        for (int neighbor : graph[currentNode]) {
            path.add(neighbor); // add neighbor to current path
            dfs(graph, neighbor, path, allPaths); // Explore path from this neighbor
            path.remove(path.size() - 1); // Backtrack: remove neighbor after exploring
        }
    }
}
```

**Time Complexity:** O(2^n * n), because in the worst case, there can be `2^n` paths, and each path can be of length `n`.

**Space Complexity:** O(n), the space used by the recursion stack.

### BFS Using a Queue

**Intuition:**
You can also solve this problem using BFS. The goal of BFS is to explore all neighbors at the present depth before moving on to nodes at the next depth level. By storing paths in the queue, at each step, when you explore a node, you construct and enqueue new paths adding each of the node's neighbors.

**Steps:**
1. Initialize a queue and add the initial path starting from node `0`.
2. While the queue is not empty, pop paths from the queue.
3. Extend each path by appending each of the current node's neighbors and enqueue the new path.
4. If a path reaches the target node `n-1`, add it to the results.
5. Continue until all paths are explored.

```java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        List<List<Integer>> allPaths = new ArrayList<>();
        Queue<List<Integer>> queue = new LinkedList<>();
        queue.add(Arrays.asList(0)); // Start with a path from node 0
        
        while (!queue.isEmpty()) {
            List<Integer> path = queue.poll();
            int currentNode = path.get(path.size() - 1);

            if (currentNode == graph.length - 1) {
                allPaths.add(new ArrayList<>(path)); // add path if it ends with target
            } else {
                for (int neighbor : graph[currentNode]) {
                    List<Integer> newPath = new ArrayList<>(path);
                    newPath.add(neighbor);
                    queue.add(newPath); // enqueue new path with neighbor
                }
            }
        }
        return allPaths;
    }
}
```

**Time Complexity:** O(2^n * n), similar to DFS, as you are exploring every possible path.

**Space Complexity:** O(2^n * n), for storing all paths in the queue.

