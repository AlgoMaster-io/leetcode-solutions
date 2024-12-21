# [Leetcode 1986: Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Approaches:
1. [Greedy Approach with Backtracking](#greedy-approach-with-backtracking)
2. [Dynamic Programming with Memoization](#dynamic-programming-with-memoization)

---

## Greedy Approach with Backtracking
The greedy approach tries to finish tasks by starting a new session if the current one is not sufficient. We use backtracking to test all possible combinations.

### Intuition
1. Start a session and try to allocate tasks until the session can't handle more tasks.
2. We recursively explore subsequent combinations of assigning remaining tasks, trying new sessions if needed.
3. This solution is feasible but not optimal due to its recursive nature, which tends to explore unnecessary paths.

### Code
```python
def minSessions(tasks, sessionTime):
    def backtrack(i, sessions):
        # Base case: if all tasks are assigned
        if i == len(tasks):
            return len(sessions)
        
        min_sessions = float('inf')
        for j in range(len(sessions)):
            if sessions[j] + tasks[i] <= sessionTime:
                sessions[j] += tasks[i]
                min_sessions = min(min_sessions, backtrack(i + 1, sessions))
                sessions[j] -= tasks[i]
        
        # Start a new session for the current task
        sessions.append(tasks[i])
        min_sessions = min(min_sessions, backtrack(i + 1, sessions))
        sessions.pop()
        
        return min_sessions
    
    return backtrack(0, [])

# Assume a sample input for clarity
print(minSessions([3, 1, 3, 8, 2], 8))  # Expected output could vary
```

### Complexity
- **Time Complexity**: O(n!) in the worst case, where n is the number of tasks. The backtracking explores every permutation.
- **Space Complexity**: O(n) for the recursion stack.

---

## Dynamic Programming with Memoization
A more optimal approach is to use dynamic programming combined with bit masking to memoize results for each state based on which tasks have been completed.

### Intuition
1. We use a bitmask to represent which tasks are completed.
2. Use dynamic programming to remember results for each state of the bitmask to avoid recomputation.
3. For each state, calculate the minimum session count by considering taking the next task in the current session or starting a new session.

### Code
```python
def minSessions(tasks, sessionTime):
    n = len(tasks)
    memo = {}
    
    def dp(mask, remaining):
        # If all tasks are completed
        if mask == (1 << n) - 1:
            return 0
        
        if (mask, remaining) in memo:
            return memo[(mask, remaining)]
        
        answer = float('inf')
        
        for i in range(n):
            if not (mask & (1 << i)):  # task i not yet done
                if tasks[i] <= remaining:
                    # Continue in the same session
                    answer = min(answer, dp(mask | (1 << i), remaining - tasks[i]))
                # Start a new session
                answer = min(answer, 1 + dp(mask | (1 << i), sessionTime - tasks[i]))
        
        memo[(mask, remaining)] = answer
        return answer

    return dp(0, sessionTime)
```

### Complexity
- **Time Complexity**: O(n * 2^n), where n is the number of tasks due to examining each state and transition.
- **Space Complexity**: O(n * 2^n) for memoization of states based on bitmask.

---

Both solutions demonstrate the trade-offs between simplicity and efficiency, with the dynamic programming approach being significantly more optimal when tasked with handling larger sets of tasks.

