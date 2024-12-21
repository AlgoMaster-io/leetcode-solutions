# [Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized DFS Approach](#optimized-dfs-approach)

---

## Brute Force Approach

### Intuition
The brute force approach is to consider calculating the sum of distances for each node in the tree individually. For each node as the root, we compute the distances from this root to all other nodes, and sum these distances. This involves performing a BFS/DFS starting from each node to compute the total distance from the node to all other nodes in the tree.

### Code
```python
def sumOfDistancesInTree(N, edges):
    from collections import defaultdict, deque
    
    # Building the adjacency list for the undirected tree.
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

    def bfs(node):
        # Set up a queue and visited set for BFS traversal
        queue = deque([node])
        visited = set([node])
        distances = {node: 0}
        
        # Total distance sum from this root
        distance_sum = 0
        
        # Perform BFS
        while queue:
            current = queue.popleft()
            for neighbor in graph[current]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
                    distances[neighbor] = distances[current] + 1
                    distance_sum += distances[neighbor]

        return distance_sum
    
    # Calculate the distance sum for each node as the root
    result = [bfs(node) for node in range(N)]
    
    return result
```

### Complexity Analysis
- **Time Complexity**: \(O(N^2)\). We perform a BFS for each node, leading to \(O(N)\) operations per node, hence \(O(N^2)\) in total.
- **Space Complexity**: \(O(N)\). For storing the graph and intermediate data structures such as the queue and visited nodes.

---

## Optimized DFS Approach

### Intuition
The optimized approach takes advantage of using two DFS traversals. The first DFS traversal calculates distances from a chosen root node to all other nodes and keeps track of subtree sizes. The second DFS uses this information to calculate distances for all nodes efficiently. By reusing calculated distances intelligently, we can achieve a much faster solution.

### Code
```python
def sumOfDistancesInTree(N, edges):
    from collections import defaultdict
    
    # Build the graph
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
        
    # Initialize arrays
    subtree_size = [0] * N
    distance_sum = [0] * N
    
    def dfs(node, parent):
        # Initial size of the subtree rooted at node is 1 (the node itself)
        subtree_size[node] = 1
        # Traverse children in graph
        for neighbor in graph[node]:
            if neighbor == parent:
                continue
            dfs(neighbor, node)
            subtree_size[node] += subtree_size[neighbor]
            distance_sum[node] += distance_sum[neighbor] + subtree_size[neighbor]
    
    def dfs2(node, parent):
        # Traverse through all children, calculating the new distance sums
        for neighbor in graph[node]:
            if neighbor == parent:
                continue
            # Adjust the distance sum for the child node
            distance_sum[neighbor] = distance_sum[node] - subtree_size[neighbor] + (N - subtree_size[neighbor])
            dfs2(neighbor, node)
    
    # First DFS to get initial distances and subtree sizes
    dfs(0, -1)
    # Second DFS to adjust distances for all nodes
    dfs2(0, -1)
    
    return distance_sum
```

### Complexity Analysis
- **Time Complexity**: \(O(N)\). Each node and edge is visited a constant number of times.
- **Space Complexity**: \(O(N)\). For storing graph structure, subtree sizes, and distance sums.

This optimized approach significantly reduces the time complexity by avoiding redundant calculations using properties of trees and reusing previously computed information.

