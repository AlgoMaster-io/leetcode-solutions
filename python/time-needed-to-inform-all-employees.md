# [Leetcode 1376: Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

## Table of Contents
1. [Approach 1: Depth First Search (DFS) - Naive Recursive Solution](#approach-1)
2. [Approach 2: Depth First Search (DFS) - Optimized with Memoization](#approach-2)
3. [Approach 3: Breadth First Search (BFS)](#approach-3)

### Approach 1: Depth First Search (DFS) - Naive Recursive Solution
In this approach, we use DFS to traverse the tree structure formed by `manager` relationships. For each employee, we recursively compute the time required to inform all of their subordinates until we reach leaf nodes (employees with no subordinates).

**Intuition:**
- Start from the head of the company, which is given by `headID`.
- For each employee, determine the time to inform all of their subordinates by summing the time it takes for this employee to inform their direct subordinates, plus the time it takes for each of those subordinates to inform their own subordinates.

**Solution:**

```python
def numOfMinutes(n, headID, manager, informTime):
    def dfs(employee_id):
        # Base case: If the employee has no subordinates
        if informTime[employee_id] == 0:
            return 0
        
        # Maximum time needed to inform all subordinates
        max_time = 0
        for subordinate, mgr in enumerate(manager):
            if mgr == employee_id:
                # Calculate time for this subordinate by a recursive call
                time_needed = dfs(subordinate)
                max_time = max(max_time, time_needed)
        
        # Total time for this employee to inform all of their subordinates
        return informTime[employee_id] + max_time
    
    # Start DFS from the headID
    return dfs(headID)
```

**Time Complexity:** 
- O(n^2) in the worst-case scenario where each employee except the last one reports directly to the head, causing repeated traversals through the list of employees.

**Space Complexity:** 
- O(n) for the recursion stack in the worst case (in a fully unbalanced tree).

### Approach 2: Depth First Search (DFS) - Optimized with Memoization
This approach improves upon the naive DFS by storing intermediate results of previously computed times for each employee, thereby avoiding redundant calculations.

**Intuition:**
- Use a memoization technique to store the results of DFS calls for individual employees, making subsequent accesses direct and efficient.

**Solution:**

```python
def numOfMinutes(n, headID, manager, informTime):
    memo = {}
    
    def dfs(employee_id):
        # Check if the result is already cached
        if employee_id in memo:
            return memo[employee_id]
        
        if informTime[employee_id] == 0:
            return 0
        
        max_time = 0
        for subordinate, mgr in enumerate(manager):
            if mgr == employee_id:
                time_needed = dfs(subordinate)
                max_time = max(max_time, time_needed)
        
        total_time = informTime[employee_id] + max_time
        memo[employee_id] = total_time
        return total_time
    
    return dfs(headID)
```

**Time Complexity:** 
- O(n), because each employee is visited only once and the results are cached.

**Space Complexity:**
- O(n) due to the stack and additional space to store computed results in the memo dictionary.

### Approach 3: Breadth First Search (BFS)
Instead of using DFS, we can apply BFS to achieve similar results. BFS can be particularly useful when trying to understand the time taken layer by layer or level by level.

**Intuition:**
- Use a queue starting with the `headID` and traverse each level, summing and updating the time required to inform each employee's subordinates.

**Solution:**

```python
from collections import deque

def numOfMinutes(n, headID, manager, informTime):
    # Build an adjacency list to represent the manager-subordinate relationships
    from collections import defaultdict
    adj = defaultdict(list)
    
    for i in range(n):
        if manager[i] != -1:
            adj[manager[i]].append(i)
    
    # BFS with a queue to store (current_employee, accumulated_time)
    queue = deque([(headID, 0)])
    max_time = 0
    
    while queue:
        current, accumulated_time = queue.popleft()
        
        # Explore all children/subordinates
        for subordinate in adj[current]:
            # Calculate new time for the subordinate
            new_time = accumulated_time + informTime[current]
            queue.append((subordinate, new_time))
            # Update the max_time
            max_time = max(max_time, new_time)
    
    return max_time
```

**Time Complexity:** 
- O(n), since each node and edge is processed once.

**Space Complexity:**
- O(n) for storing the adjacency list and the queue.

These methods provide a range of solutions from simple recursive DFS to optimized and level-wise BFS, ensuring understanding from a fundamental to an advanced level of solving the problem.

