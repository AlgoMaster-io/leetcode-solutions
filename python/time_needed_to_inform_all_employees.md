# 1376. [Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

## Approach: Depth-First Search (DFS)

### Solution
python
```python
# Time Complexity: O(n), where n is the number of employees
# Space Complexity: O(n), for the adjacency list and recursion stack

class Solution:
    def numOfMinutes(self, n: int, headID: int, manager: List[int], informTime: List[int]) -> int:
        from collections import defaultdict

        # Step 1: Build an adjacency list for the employee-manager hierarchy
        adj_list = defaultdict(list)
        for i in range(n):
            if manager[i] != -1:
                adj_list[manager[i]].append(i)

        # Step 2: Perform DFS to calculate the total time to inform all employees
        def dfs(current: int) -> int:
            max_time = 0

            # Recur for all subordinates of the current employee
            for subordinate in adj_list[current]:
                max_time = max(max_time, dfs(subordinate))

            # Total time is the current employee's inform time + max time of subordinates
            return informTime[current] + max_time

        return dfs(headID)
```

