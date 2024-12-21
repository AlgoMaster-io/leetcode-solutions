# [Leetcode 210: Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

## Approaches
- [Approach 1: DFS with Backtracking](#approach-1-dfs-with-backtracking)
- [Approach 2: Kahn's Algorithm for Topological Sorting (BFS)](#approach-2-kahns-algorithm-for-topological-sorting-bfs)

---

## Approach 1: DFS with Backtracking

### Intuition
The problem of determining the order of courses to take can be transformed into finding a topological order of a directed graph. Each course is represented as a node, and each prerequisite as a directed edge. The idea is that if you perform a Depth-First Search (DFS) on the graph, you can detect cycles, and simultaneously construct the topological sorting order by backtracking.

### Implementation

```cpp
#include <vector>
#include <stack>

using namespace std;

class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        // Adjacency list representation of the graph
        vector<vector<int>> adjList(numCourses);
        for (const auto& pair : prerequisites) {
            adjList[pair[1]].push_back(pair[0]);
        }
        
        // Vector to track visited nodes (0=not visited, 1=visiting, 2=visited)
        vector<int> visited(numCourses, 0);
        stack<int> orderStack;  // Stack to maintain the topological order
        bool possible = true;

        // Helper function for DFS
        function<void(int)> dfs = [&](int node) {
            if (!possible) return;

            visited[node] = 1; // Mark the node as visiting
            for (int neighbor : adjList[node]) {
                if (visited[neighbor] == 0) {
                    dfs(neighbor);
                } else if (visited[neighbor] == 1) {
                    possible = false; // Found a cycle
                }
            }
            visited[node] = 2; // Mark the node as visited
            orderStack.push(node); // Add node to topological order
        };
        
        // Perform DFS for each node
        for (int i = 0; i < numCourses; ++i) {
            if (visited[i] == 0 && possible) {
                dfs(i);
            }
        }
        
        // If we detected a cycle, return an empty array
        if (!possible) return {};
        
        // Extract elements from the stack
        vector<int> order;
        while (!orderStack.empty()) {
            order.push_back(orderStack.top());
            orderStack.pop();
        }
        
        return order;
    }
};
```

### Complexity
- **Time Complexity:** \(O(V + E)\) where \(V\) is the number of courses and \(E\) is the number of prerequisites. We visit each node and edge.
- **Space Complexity:** \(O(V)\), for the recursion stack and the visited array.

---

## Approach 2: Kahn's Algorithm for Topological Sorting (BFS)

### Intuition
Kahn's algorithm is another popular method for topological sorting using BFS. The core idea is to iteratively remove nodes with no incoming edges (in-degree = 0) and reduce the in-degrees of their neighbors. This effectively builds the topological order layer by layer.

### Implementation

```cpp
#include <vector>
#include <queue>

using namespace std;

class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        // Adjacency list representation of the graph
        vector<vector<int>> adjList(numCourses);
        // Array to track indegrees of each node
        vector<int> inDegree(numCourses, 0);

        for (const auto& pair : prerequisites) {
            adjList[pair[1]].push_back(pair[0]);
            ++inDegree[pair[0]];
        }

        queue<int> zeroInDegreeQueue; // Queue of courses with zero in-degree
        for (int i = 0; i < numCourses; ++i) {
            if (inDegree[i] == 0) {
                zeroInDegreeQueue.push(i);
            }
        }

        vector<int> order;
        while (!zeroInDegreeQueue.empty()) {
            int node = zeroInDegreeQueue.front();
            zeroInDegreeQueue.pop();
            order.push_back(node);

            // For each neighbor, reduce the in-degree by 1
            for (int neighbor : adjList[node]) {
                --inDegree[neighbor];
                // If in-degree is now 0, add it to the queue
                if (inDegree[neighbor] == 0) {
                    zeroInDegreeQueue.push(neighbor);
                }
            }
        }

        // If we collected all courses in the order, return it, otherwise return empty array
        return (order.size() == numCourses) ? order : vector<int>{};
    }
};
```

### Complexity
- **Time Complexity:** \(O(V + E)\), where \(V\) is the number of courses and \(E\) is the number of prerequisites. We visit each node and process each edge.
- **Space Complexity:** \(O(V + E)\), for the adjacency list and the in-degree array.

