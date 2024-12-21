# [Leetcode 133: Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approaches
- [Approach 1: Depth First Search (DFS) - Recursive Solution](#approach-1-depth-first-search-dfs---recursive-solution)
- [Approach 2: Breadth First Search (BFS)](#approach-2-breadth-first-search-bfs)

### Approach 1: Depth First Search (DFS) - Recursive Solution

#### Intuition:
The goal is to create a deep copy of an undirected graph where each node is a copy of the original, maintaining the same neighbor relationships. A depth-first search can be employed where we recursively copy each node and its neighbors using a hash map to keep track of already copied nodes to avoid redundant work and infinite loops.

#### Detailed Description:
1. Use a hashmap to store the original node as the key and its clone as the value. This helps in quickly finding if a node is already cloned.
2. Start the recursion with the given node. If the node is nil, directly return nil.
3. Check if the node is already cloned by looking it up in the hashmap. If cloned, return the cloned node.
4. If not, create a new node as a clone of the current node and add it to the hashmap.
5. Recursively clone all the neighbors of the node.
6. Return the cloned node.

#### Code:

```go
type Node struct {
    Val       int
    Neighbors []*Node
}

func cloneGraph(node *Node) *Node {
    if node == nil {
        return nil
    }

    // A map to save the visited nodes
    visited := make(map[*Node]*Node)

    // DFS recursive function
    var clone func(node *Node) *Node
    clone = func(node *Node) *Node {
        // Check if the node is already cloned
        if n, found := visited[node]; found {
            return n
        }

        // Create a new node for the clone
        cloneNode := &Node{Val: node.Val, Neighbors: []*Node{}}
        // Save this node to visited
        visited[node] = cloneNode

        // Iterate through the neighbors to clone them recursively
        for _, neighbor := range node.Neighbors {
            cloneNode.Neighbors = append(cloneNode.Neighbors, clone(neighbor))
        }

        return cloneNode
    }

    return clone(node)
}
```

#### Time Complexity:
- O(N + M), where N is the number of nodes and M is the number of edges. Each node and edge is considered once.

#### Space Complexity:
- O(N), for the recursive function call stack and the hashmap which stores the node mapping.

### Approach 2: Breadth First Search (BFS)

#### Intuition:
Instead of using a recursive approach to traverse, use BFS to iteratively explore and clone the graph level by level. BFS allows us to clone the graph in a breadthwards fashion starting from the given input node.

#### Detailed Description:
1. Use a hashmap to store the original node as the key and its clone as the value.
2. Initialize a queue to assist with BFS, begin by pushing the start node into the queue.
3. As you pop a node from the queue, clone it, and check if it already exists in the map.
4. Clone all the neighbors of this node, and add them to the queue if they haven't been visited yet.
5. Continue this process until the queue is empty.

#### Code:

```go
func cloneGraph(node *Node) *Node {
    if node == nil {
        return nil
    }

    // Map to keep track of cloned nodes
    cloneMap := make(map[*Node]*Node)
    queue := []*Node{node}
    
    // Clone the root node
    cloneMap[node] = &Node{Val: node.Val}
    
    // BFS traversal
    for len(queue) > 0 {
        // Dequeue the node
        n := queue[0]
        queue = queue[1:]

        // Iterate through each neighbor
        for _, neighbor := range n.Neighbors {
            if _, exists := cloneMap[neighbor]; !exists {
                // Clone the neighbor node
                cloneMap[neighbor] = &Node{Val: neighbor.Val}
                // Add unvisited node into the queue
                queue = append(queue, neighbor)
            }
            // Link the clone of the neighbors to the clone node
            cloneMap[n].Neighbors = append(cloneMap[n].Neighbors, cloneMap[neighbor])
        }
    }
    
    return cloneMap[node]
}
```

#### Time Complexity:
- O(N + M), where N is the number of nodes and M is the number of edges. Each node and each edge are processed exactly once.

#### Space Complexity:
- O(N), for maintaining the hashmap and the queue.

