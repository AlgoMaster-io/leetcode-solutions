# [Leetcode 133: Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approaches
- [Approach 1: Depth-First Search (Recursive)](#approach-1-depth-first-search-recursive)
- [Approach 2: Breadth-First Search (Iterative)](#approach-2-breadth-first-search-iterative)

## Approach 1: Depth-First Search (Recursive)

### Intuition
To clone an undirected graph, we can use a Depth-First Search (DFS) approach. The idea is to maintain a mapping from each original node to its copy. Starting from an arbitrary node, for each node visited, we'll:
- Clone the current node (if not already cloned).
- Clone all its neighbors using DFS recursively.

By storing all cloned nodes in a hashmap, we prevent duplicates and ensure every node is only cloned once.

### Code
```typescript
class Node {
    val: number
    neighbors: Node[]
    constructor(val?: number, neighbors?: Node[]) {
        this.val = (val === undefined ? 0 : val)
        this.neighbors = (neighbors === undefined ? [] : neighbors)
    }
}

function cloneGraph(node: Node | null): Node | null {
    if (!node) return null;

    // Map to store original node to cloned node mapping
    const clonedNodesMap: Map<Node, Node> = new Map();

    // Recursive DFS function
    function dfs(node: Node): Node {
        // If the node is already cloned, return its clone
        if (clonedNodesMap.has(node)) {
            return clonedNodesMap.get(node)!;
        }

        // Clone the node
        const clonedNode = new Node(node.val);
        clonedNodesMap.set(node, clonedNode);

        // Recursively clone all the neighbors
        for (let neighbor of node.neighbors) {
            clonedNode.neighbors.push(dfs(neighbor));
        }

        return clonedNode;
    }

    // Start DFS from the given node
    return dfs(node);
}
```

### Complexity Analysis
- **Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges. Each node and edge is traversed once.
- **Space Complexity:** O(V), for storing the cloned nodes in the map.

## Approach 2: Breadth-First Search (Iterative)

### Intuition
The BFS approach involves iteratively traversing the graph level by level. We use a queue for the traversal and a hashmap to store cloned node references. The operations are as follows:
- Clone the first node, put it in the queue, and map it.
- While the queue is not empty:
  - Dequeue a node and iterate over its neighbors.
  - For each neighbor, clone it if not already cloned, map it, enqueue the original neighbor, and link the clone neighbors.

### Code
```typescript
function cloneGraphBFS(node: Node | null): Node | null {
    if (!node) return null;

    // Map to store original node to cloned node mapping
    const clonedNodesMap: Map<Node, Node> = new Map();

    // Initialize queue for BFS
    const queue: Node[] = [node];

    // Clone the root node
    clonedNodesMap.set(node, new Node(node.val));

    // BFS traversal of the graph
    while (queue.length > 0) {
        const currentNode = queue.shift()!;

        // Traverse all its neighbors
        for (let neighbor of currentNode.neighbors) {
            // If the neighbor is not cloned yet
            if (!clonedNodesMap.has(neighbor)) {
                // Clone it and map it
                clonedNodesMap.set(neighbor, new Node(neighbor.val));
                // Add it to the queue for further processing
                queue.push(neighbor);
            }
            // Link the clone of the current node with the clone of the neighbor
            clonedNodesMap.get(currentNode)!.neighbors.push(clonedNodesMap.get(neighbor)!);
        }
    }

    // Return the clone of the start node
    return clonedNodesMap.get(node)!;
}
```

### Complexity Analysis
- **Time Complexity:** O(V + E), similar to DFS as each node and edge is processed once.
- **Space Complexity:** O(V), which is used by the queue and hashmap to store nodes and their clones.

