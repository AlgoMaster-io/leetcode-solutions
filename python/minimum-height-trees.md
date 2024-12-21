# [Leetcode 310: Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

## Approaches
1. [Basic Solution: Brute Force with BFS on each node](#basic-solution-brute-force-with-bfs-on-each-node)
2. [Optimized Solution: Trim Leaves Iteratively](#optimized-solution-trim-leaves-iteratively)
3. [Most Optimal Solution: Using Degree and Central Concept](#most-optimal-solution-using-degree-and-central-concept)

### Basic Solution: Brute Force with BFS on each node

#### Intuition
The brute force method involves attempting to find the minimum height tree by selecting each node as the root and then calculating the maximum height of the tree. This involves running a BFS or DFS from each node, computing the height, and then finding the minimum height among these.

#### Approach
1. For each node, perform a BFS to determine the height when this node serves as the root.
2. Compare the heights and keep track of the minimum height and the corresponding nodes.
3. This is inefficient as it performs a traversal for each node.

#### Code
```python
def findMinHeightTrees(n, edges):
    if n <= 2:
        return [i for i in range(n)]
    
    from collections import defaultdict, deque
    
    def bfs(root):
        queue = deque([root])
        visited = [False] * n
        visited[root] = True
        level = 0
        
        while queue:
            size = len(queue)
            for _ in range(size):
                node = queue.popleft()
                for neighbor in graph[node]:
                    if not visited[neighbor]:
                        visited[neighbor] = True
                        queue.append(neighbor)
            level += 1
        return level - 1
    
    # Building the graph
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
    
    minHeight = float('inf')
    result = []
    
    # Calculate height for each node
    for i in range(n):
        height = bfs(i)
        if height < minHeight:
            minHeight = height
            result = [i]
        elif height == minHeight:
            result.append(i)
    
    return result
```

#### Complexity
- **Time Complexity**: \(O(N^2)\) because we traverse each node to compute the tree height.
- **Space Complexity**: \(O(N)\) for storing visited nodes and graph representation.

---

### Optimized Solution: Trim Leaves Iteratively

#### Intuition
Unlike the brute force strategy, by leveraging the properties of a tree, we know that the MHT root can be found by iteratively removing leaves. The intuition is similar to peeling a tree from its outermost leaves and reaching the core where nodes with minimum degrees exist.

#### Approach
1. Initialize a queue with all leaf nodes.
2. Remove leaves iteratively and update the tree.
3. The process will eventually converge to the center(s) of the tree.

#### Code
```python
def findMinHeightTrees(n, edges):
    if n <= 2:
        return [i for i in range(n)]
    
    from collections import deque, defaultdict
    
    # Build the graph
    graph = defaultdict(set)
    for u, v in edges:
        graph[u].add(v)
        graph[v].add(u)
    
    # Initialize leaves (nodes with only one connection)
    leaves = deque([i for i in range(n) if len(graph[i]) == 1])
    
    # Trim the leaves until we reach the roots of MHTs
    remaining_nodes = n
    while remaining_nodes > 2:
        leaves_count = len(leaves)
        remaining_nodes -= leaves_count
        
        for _ in range(leaves_count):
            leaf = leaves.popleft()
            # There will only be one neighbor for a leaf
            neighbor = graph[leaf].pop()
            graph[neighbor].remove(leaf)
            
            if len(graph[neighbor]) == 1:
                leaves.append(neighbor)
    
    return list(leaves)
```

#### Complexity
- **Time Complexity**: \(O(N)\) we process each edge and node once.
- **Space Complexity**: \(O(N)\) for storing the degrees and the graph.

---

### Most Optimal Solution: Using Degree and Central Concept

#### Intuition
This solution is similar to optimizing topological sorting techniques. The root of a minimum height tree (MHT) must be the central node(s) which are the last nodes left when all leaves are stripped away iteratively. Utilizing the degree of each node, nodes are processed in rounds reducing nodes in the process.

#### Code
```python
def findMinHeightTrees(n, edges):
    if n <= 2:
        return [i for i in range(n)]
    
    from collections import defaultdict, deque
    
    # Build the graph
    graph = defaultdict(set)
    for u, v in edges:
        graph[u].add(v)
        graph[v].add(u)
    
    # Identify initial leaves
    leaves = deque([i for i in range(n) if len(graph[i]) == 1])
    
    remaining_nodes = n
    while remaining_nodes > 2:
        leaves_count = len(leaves)
        remaining_nodes -= leaves_count
        
        for _ in range(leaves_count):
            leaf = leaves.popleft()
            neighbor = graph[leaf].pop()
            graph[neighbor].remove(leaf)
            
            if len(graph[neighbor]) == 1:
                leaves.append(neighbor)
    
    # The remaining nodes are the centroids of the graph
    return list(leaves)
```

#### Complexity
- **Time Complexity**: \(O(N)\), we efficiently reduce by processing each node and edge once.
- **Space Complexity**: \(O(N)\) for storing graph and degrees.

