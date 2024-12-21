# [Leetcode 1376: Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

## Approaches
- [Approach 1: Depth First Search (DFS) with Adjacency List](#approach-1-depth-first-search-dfs-with-adjacency-list)
- [Approach 2: Breadth First Search (BFS) with Queue](#approach-2-breadth-first-search-bfs-with-queue)

---

## Approach 1: Depth First Search (DFS) with Adjacency List

### Intuition
The problem can be visualized as a tree, where each employee can be seen as a node. The manager of each employee can be seen as an edge connecting nodes. The task is to determine how long it takes in total to inform all employees starting from the given headID. This can be efficiently done via DFS by calculating the maximum time needed to inform all subordinate chains of each employee.

### Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    int dfs(int employee, vector<vector<int>>& adjList, vector<int>& informTime) {
        int maxTime = 0;
        for (int child : adjList[employee]) {
            // Recursively find the needed time for each subordinate
            maxTime = max(maxTime, dfs(child, adjList, informTime));
        }
        return informTime[employee] + maxTime;
    }
    
    int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime) {
        vector<vector<int>> adjList(n);
        
        // Create adjacency list representation of the company
        for (int i = 0; i < n; ++i) {
            if (manager[i] != -1) { // do not add the head of the company
                adjList[manager[i]].push_back(i);
            }
        }
        
        // Start the DFS from the head of the company
        return dfs(headID, adjList, informTime);
    }
};
```

### Time Complexity
- O(N), where N is the number of employees, since we visit each node once.
  
### Space Complexity
- O(N), due to the space used for the adjacency list representation of the company hierarchy.

---

## Approach 2: Breadth First Search (BFS) with Queue

### Intuition
Instead of using DFS, we can leverage BFS to perform a level-order traversal, starting from the head of the company. For each employee, we send the information to all subordinates and accumulate the time needed to inform them. BFS will help in ensuring we expand each level of the tree completely before moving deeper.

### Code
```cpp
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

class Solution {
public:
    int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime) {
        vector<vector<int>> adjList(n);
        
        // Create adjacency list: manager[i] is the manager of employee i
        for (int i = 0; i < n; ++i) {
            if (manager[i] != -1) {  // do not add the head of the company
                adjList[manager[i]].push_back(i);
            }
        }
        
        queue<pair<int, int>> q; // pair of (employee, time taken to reach)
        q.push({headID, 0});
        
        int maxTime = 0;
        
        while (!q.empty()) {
            int employee = q.front().first;
            int currentTime = q.front().second;
            q.pop();
            
            maxTime = max(maxTime, currentTime);
            
            for (int subordinate : adjList[employee]) {
                q.push({subordinate, currentTime + informTime[employee]});
            }
        }
        
        return maxTime;
    }
};
```

### Time Complexity
- O(N), where N is the number of employees, since we visit each node exactly once in the queue processing.

### Space Complexity
- O(N), to store the adjacency list and any extra space used by the queue. 

Each solution provides a distinct method to achieve the desired result; the DFS approach focuses on depth calculation, while the BFS approach systematically explores each level of hierarchy.

