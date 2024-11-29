# 1376. [Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

## Approach: Depth-First Search (DFS)

### Solution
```java
// Time Complexity: O(n), where n is the number of employees
// Space Complexity: O(n), for the adjacency list and recursion stack
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int numOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        // Step 1: Build an adjacency list for the employee-manager hierarchy
        List<List<Integer>> adjList = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adjList.add(new ArrayList<>());
        }

        for (int i = 0; i < n; i++) {
            if (manager[i] != -1) {
                adjList.get(manager[i]).add(i);
            }
        }

        // Step 2: Perform DFS to calculate the total time to inform all employees
        return dfs(headID, adjList, informTime);
    }

    private int dfs(int current, List<List<Integer>> adjList, int[] informTime) {
        int maxTime = 0;

        // Recur for all subordinates of the current employee
        for (int subordinate : adjList.get(current)) {
            maxTime = Math.max(maxTime, dfs(subordinate, adjList, informTime));
        }

        // Total time is the current employee's inform time + max time of subordinates
        return informTime[current] + maxTime;
    }
}
```