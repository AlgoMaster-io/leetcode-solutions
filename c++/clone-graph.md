# [Leetcode 133: Clone Graph](https://leetcode.com/problems/clone-graph/)

## Approaches
1. [Depth-First Search (DFS) using Recursion](#approach-1)
2. [Breadth-First Search (BFS) using Queue](#approach-2)

## Approach 1: Depth-First Search (DFS) using Recursion

### Intuition
The problem requires creating a copy of an undirected graph. A straightforward way to traverse the graph and clone it is to use depth-first search (DFS). We can recursively visit each node in the graph. As we visit each node, we create a new cloned node and keep a mapping from the original node to the clone node. For every neighbor, if it hasn't been cloned yet, we recursively clone the neighbor and add it to our current node's neighbors.

### Steps
1. Initialize a hash map to store already cloned nodes.
2. For each node, if it has been cloned (exists in the map), return the cloned node.
3. Else, create a clone of the node.
4. Add each neighbor recursively using DFS.
5. Return the cloned node.

### Code
```cpp
class Node {
public:
    int val;
    vector<Node*> neighbors;
    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }
    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }
    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};

class Solution {
    unordered_map<Node*, Node*> visited;

    Node* dfs(Node* node) {
        // If the node is nullptr, return nullptr
        if (!node) return nullptr;

        // If the node is already visited, return the visited node
        if (visited.count(node)) return visited[node];

        // Create a clone for the current node
        Node* cloneNode = new Node(node->val);
        
        // Mark this node as visited by adding it to the map
        visited[node] = cloneNode;

        // Iterate over the neighbors to clone them
        for (auto neighbor : node->neighbors) {
            cloneNode->neighbors.push_back(dfs(neighbor));
        }
        
        return cloneNode;
    }

public:
    Node* cloneGraph(Node* node) {
        return dfs(node);
    }
};
```

### Complexity Analysis
- **Time Complexity**: \(O(V + E)\), where \(V\) is the number of nodes and \(E\) is the number of edges. Each node and each edge is visited once.
- **Space Complexity**: \(O(V)\), for storing the cloned nodes in the map and recursive stack space.

## Approach 2: Breadth-First Search (BFS) using Queue

### Intuition
In BFS, we utilize a queue to iterate level-by-level. This technique is suitable when needing to handle each node and its direct neighbors distinctly, much like waves expanding. BFS aids in incrementally generating the graph by replacing recursive recursion with iterative looping.

### Steps
1. Initialize a queue for BFS and hash map to store cloned nodes.
2. Clone the first node and offer it to the queue.
3. While the queue is not empty, process each node:
   - Get the node from the queue and traverse all its neighbors.
   - If a neighbor has not been cloned, clone it and queue the original neighbor.
   - Link the neighbor to the current node's clone.

### Code
```cpp
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if (!node) return nullptr;

        // Hash map to store visited nodes
        unordered_map<Node*, Node*> visited;

        // Queue for BFS
        queue<Node*> q;
        
        // Clone the root node
        Node* clone = new Node(node->val);
        visited[node] = clone;
        q.push(node);

        while (!q.empty()) {
            Node* current = q.front();
            q.pop();
            
            // Iterate through neighbors
            for (auto neighbor : current->neighbors) {
                if (!visited.count(neighbor)) {
                    // Clone the neighbor node if it hasn't been cloned yet
                    visited[neighbor] = new Node(neighbor->val);
                    // Push it onto the queue
                    q.push(neighbor);
                }
                // Link the cloned neighbor to the cloned current node
                visited[current]->neighbors.push_back(visited[neighbor]);
            }
        }

        return clone;
    }
};
```

### Complexity Analysis
- **Time Complexity**: \(O(V + E)\), with \(V\) being nodes, \(E\) edges, similar to the DFS approach as each node and edge is processed once.
- **Space Complexity**: \(O(V)\), for both the hash map and the queue used for storing the nodes.


