# 1376. [Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

## Approach: Depth-First Search (DFS)

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of employees
// Space Complexity: O(n), for the adjacency list and recursion stack
using System;
using System.Collections.Generic;

public class Solution {
    public int NumOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        // Step 1: Build an adjacency list for the employee-manager hierarchy
        List<List<int>> adjList = new List<List<int>>();
        for (int i = 0; i < n; i++) {
            adjList.Add(new List<int>());
        }

        for (int i = 0; i < n; i++) {
            if (manager[i] != -1) {
                adjList[manager[i]].Add(i);
            }
        }

        // Step 2: Perform DFS to calculate the total time to inform all employees
        return Dfs(headID, adjList, informTime);
    }

    private int Dfs(int current, List<List<int>> adjList, int[] informTime) {
        int maxTime = 0;

        // Recur for all subordinates of the current employee
        foreach (int subordinate in adjList[current]) {
            maxTime = Math.Max(maxTime, Dfs(subordinate, adjList, informTime));
        }

        // Total time is the current employee's inform time + max time of subordinates
        return informTime[current] + maxTime;
    }
}
```

