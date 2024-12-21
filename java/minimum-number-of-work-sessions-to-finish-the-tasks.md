# [Leetcode 1986: Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Approaches

- [Approach 1: Backtracking](#approach-1-backtracking)
- [Approach 2: Dynamic Programming with State Compression](#approach-2-dynamic-programming-with-state-compression)

---

## Approach 1: Backtracking

In this approach, we will use simple backtracking. The basic idea is to try all possible ways to assign tasks to work sessions and keep track of the minimum number of sessions needed.

### Intuition

1. We initially start with an empty assignment of tasks to work sessions.
2. For each task, we have several options:
   - Start a new session and assign the task to this session.
   - Assign the task to an existing session if its duration plus this task's duration does not exceed the session's limit.
3. We explore all possibilities using backtracking, and at each step, we attempt to assign a task either to a new session or a suitable existing session.
4. Base Case: When all the tasks are assigned, count the number of sessions used and record the result if it's the minimum so far.

### Code

```java
class Solution {
    private int minSessions = Integer.MAX_VALUE;
    
    public int minSessions(int[] tasks, int sessionTime) {
        backtrack(tasks, sessionTime, new ArrayList<>(), 0);
        return minSessions;
    }
    
    private void backtrack(int[] tasks, int sessionTime, List<Integer> sessions, int taskIndex) {
        if (taskIndex == tasks.length) {
            minSessions = Math.min(minSessions, sessions.size());
            return;
        }
        
        for (int i = 0; i < sessions.size(); i++) {
            if (sessions.get(i) + tasks[taskIndex] <= sessionTime) {
                // Assign task to an existing session
                sessions.set(i, sessions.get(i) + tasks[taskIndex]);
                backtrack(tasks, sessionTime, sessions, taskIndex + 1);
                // Backtrack, remove the task from the session
                sessions.set(i, sessions.get(i) - tasks[taskIndex]);
            }
        }
        
        // Start a new session with the current task
        sessions.add(tasks[taskIndex]);
        backtrack(tasks, sessionTime, sessions, taskIndex + 1);
        // Backtrack by removing the new session
        sessions.remove(sessions.size() - 1);
    }
}
```

### Time and Space Complexity

- **Time Complexity:** O(n!), where n is the number of tasks. This is because of all the possible permutations of task assignments.
- **Space Complexity:** O(n), for the recursive call stack and additional space used by the `sessions`.

---

## Approach 2: Dynamic Programming with State Compression

This approach improves efficiency by using dynamic programming combined with bitmasking to represent task assignments in sessions.

### Intuition

1. Use a bitmask to represent subsets of tasks and dynamic programming to store the minimum sessions required for each subset.
2. For a given state (submask of tasks), split it into two parts representing assignments to two different groups of sessions.
3. Calculate the minimum sessions required for each group and update the state.
4. Iterate all subsets and use previously computed results to find the minimum sessions needed.

### Code

```java
class Solution {
    public int minSessions(int[] tasks, int sessionTime) {
        int n = tasks.length;
        int maxMask = (1 << n);
        int[] minSessions = new int[maxMask];
        
        // Initialize with a large value
        Arrays.fill(minSessions, Integer.MAX_VALUE);
        minSessions[0] = 0; // Base case: no tasks need zero sessions
        
        for (int mask = 0; mask < maxMask; mask++) {
            int workSessionTime = 0;
            for (int i = 0; i < n; i++) {
                if ((mask & (1 << i)) != 0) {
                    workSessionTime += tasks[i];
                }
            }

            if (workSessionTime <= sessionTime) {
                // Calculate new states
                for (int submask = mask; submask != 0; submask = (submask - 1) & mask) {
                    int subWorkTime = 0;
                    for (int i = 0; i < n; i++) {
                        if ((submask & (1 << i)) != 0) {
                            subWorkTime += tasks[i];
                        }
                    }
                    if (subWorkTime <= sessionTime) {
                        minSessions[mask] = Math.min(minSessions[mask], minSessions[mask ^ submask] + 1);
                    }
                }
            }
        }
        return minSessions[maxMask - 1];
    }
}
```

### Time and Space Complexity

- **Time Complexity:** O(2^n * n), where n is the number of tasks. Each state is visited, and for each state, we explore its subsets.
- **Space Complexity:** O(2^n), for storing results of each submask in the dynamic programming array.

---

By structuring the solution this way, you can provide a clear and effective explanation of different approaches to solve the problem, catering to both beginners with a simple backtracking approach and advanced users with a more optimal dynamic programming solution using state compression.

