# [Leetcode 797: All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## Table of Contents
- [Approach 1: Backtracking](#approach-1-backtracking)
- [Approach 2: Depth First Search using Stack](#approach-2-depth-first-search-using-stack)
- [Approach 3: Breadth First Search using Queue](#approach-3-breadth-first-search-using-queue)

## Approach 1: Backtracking

### Intuition:
This problem can be solved using backtracking, where we explore each path from the source to the target recursively. We start from the source node, attempt to traverse all possible paths, and backtrack once a dead end or the target node is reached. This approach closely resembles a Depth First Search (DFS) strategy.

### Detailed Steps:
1. Start from the source node (0) and initiate a path containing this node.
2. For each node, recursively explore all its neighboring nodes.
3. If the target node is reached, add the current path to the result.
4. Backtrack: Remove the last node from the current path and explore other remaining paths from the previous nodes.

### Code:
```cpp
class Solution {
public:
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        vector<vector<int>> result;
        vector<int> path;

        // Start backtracking from the source node (0)
        backtrack(graph, 0, path, result);
        
        return result;
    }
    
private:
    void backtrack(vector<vector<int>>& graph, int node, vector<int>& path, vector<vector<int>>& result) {
        path.push_back(node); // Add current node to path

        // Check if current node is the target node
        if (node == graph.size() - 1) {
            result.push_back(path); // Add current path to result
        } else {
            // Explore each neighboring node
            for (int nextNode : graph[node]) {
                backtrack(graph, nextNode, path, result);
            }
        }

        // Backtrack: Remove current node from path
        path.pop_back();
    }
};
```
### Complexity:
- **Time Complexity**: O(2^V) - In the worst-case scenario, we could check all possible subsets of the nodes.
- **Space Complexity**: O(V) - Due to the recursion stack and path storage.

## Approach 2: Depth First Search using Stack

### Intuition:
Instead of using the recursive approach, the same DFS-like traversal can be accomplished using an iterative approach with a stack. This is an iterative simulation of recursive backtracking.

### Detailed Steps:
1. Initialize a stack to store pairs of a node and its path.
2. Start with the stack containing only the source node and path.
3. While the stack is not empty:
   - Pop a node and its path from the stack.
   - If the node is the target, add the path to the result.
   - Otherwise, push all neighbors with the updated path back to the stack.

### Code:
```cpp
class Solution {
public:
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        vector<vector<int>> result;
        stack<pair<int, vector<int>>> s;
        
        // Start from node 0
        s.push({0, {0}});
        
        while (!s.empty()) {
            auto [node, path] = s.top();
            s.pop();

            // Check if current node is the target node
            if (node == graph.size() - 1) {
                result.push_back(path);
            } else {
                // Push each neighbor node to the stack with updated path
                for (int nextNode : graph[node]) {
                    vector<int> newPath = path;
                    newPath.push_back(nextNode);
                    s.push({nextNode, newPath});
                }
            }
        }
        
        return result;
    }
};
```
### Complexity:
- **Time Complexity**: O(2^V) - As we are exploring each path.
- **Space Complexity**: O(V) - For storing paths in the stack.

## Approach 3: Breadth First Search using Queue

### Intuition:
The BFS approach involves level-wise exploration of paths using a queue. This ensures that all paths of a certain length are considered before exploring longer paths.

### Detailed Steps:
1. Initialize a queue storing pairs of current node and path traversed.
2. Start with the source node in the queue.
3. Process each element of the queue:
   - If the node is the target, add the path to the result.
   - Otherwise, enqueue each neighbor with the updated path.
   
### Code:
```cpp
class Solution {
public:
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        vector<vector<int>> result;
        queue<pair<int, vector<int>>> q;
        
        // Start from node 0
        q.push({0, {0}});
        
        while (!q.empty()) {
            auto [node, path] = q.front();
            q.pop();

            // Check if current node is the target node
            if (node == graph.size() - 1) {
                result.push_back(path);
            } else {
                // Enqueue each neighbor node with updated path
                for (int nextNode : graph[node]) {
                    vector<int> newPath = path;
                    newPath.push_back(nextNode);
                    q.push({nextNode, newPath});
                }
            }
        }
        
        return result;
    }
};
```
### Complexity:
- **Time Complexity**: O(2^V) - Similarly exploring all possible paths.
- **Space Complexity**: O(V) - For the queue storage of paths.

In all approaches, the exploration of all possible paths is due to the nature of the problem, making the primary difference the method of traversal (recursive vs. iterative using a stack or queue).

