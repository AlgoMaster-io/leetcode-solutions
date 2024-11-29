# 1376. [Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

## Approach: Depth-First Search (DFS)

### Solution
```cpp
// Time Complexity: O(n), where n is the number of employees
// Space Complexity: O(n), for the adjacency list and recursion stack
#include <vector>
#include <algorithm>

class Solution {
public:
    int numOfMinutes(int n, int headID, std::vector<int>& manager, std::vector<int>& informTime) {
        // Step 1: Build an adjacency list for the employee-manager hierarchy
        std::vector<std::vector<int>> adjList(n);

        for (int i = 0; i < n; ++i) {
            if (manager[i] != -1) {
                adjList[manager[i]].push_back(i);
            }
        }

        // Step 2: Perform DFS to calculate the total time to inform all employees
        return dfs(headID, adjList, informTime);
    }

private:
    int dfs(int current, std::vector<std::vector<int>>& adjList, std::vector<int>& informTime) {
        int maxTime = 0;

        // Recur for all subordinates of the current employee
        for (int subordinate : adjList[current]) {
            maxTime = std::max(maxTime, dfs(subordinate, adjList, informTime));
        }

        // Total time is the current employee's inform time + max time of subordinates
        return informTime[current] + maxTime;
    }
};
```

