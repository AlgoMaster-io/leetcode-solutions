# 133. [Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approach: Depth-First Search (DFS) with HashMap

### Solution
go
```go
// Time Complexity: O(n), where n is the number of nodes in the graph
// Space Complexity: O(n), for the map and recursion stack
type Node struct {
    Val       int
    Neighbors []*Node
}

func cloneGraph(node *Node) *Node {
    if node == nil {
        return nil
    }

    // Use a map to track copied nodes
    nodeMap := make(map[*Node]*Node)
    return cloneHelper(node, nodeMap)
}

func cloneHelper(node *Node, nodeMap map[*Node]*Node) *Node {
    // If the node is already cloned, return the cloned node
    if clonedNode, found := nodeMap[node]; found {
        return clonedNode
    }

    // Clone the current node
    clone := &Node{Val: node.Val}
    nodeMap[node] = clone

    // Recursively clone all neighbors
    for _, neighbor := range node.Neighbors {
        clone.Neighbors = append(clone.Neighbors, cloneHelper(neighbor, nodeMap))
    }

    return clone
}
```

