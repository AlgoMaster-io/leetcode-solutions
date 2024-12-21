# [Leetcode 1986: Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Approaches:
- [Brute Force with Backtracking](#brute-force-with-backtracking)
- [Dynamic Programming with State Compression](#dynamic-programming-with-state-compression)

### Brute Force with Backtracking

**Intuition:**
The basic idea is to try every possible way to distribute tasks across work sessions and find the minimum number of sessions needed. This approach relies on backtracking: we make a choice to assign a task to a session, continue with that choice, and backtrack if necessary.

1. **Choose a task**: Start by taking a task and try to place it in an existing session if possible.
2. **Create a new session if needed**: If the task cannot fit into any existing session, create a new session.
3. **Recursion**: Continue this process for all tasks using recursion.
4. **Memoization**: Use memoization to avoid redundant calculations and speed up the process.

**Time Complexity:** The time complexity is O((n^n) * n) where n is the number of tasks since there are n! ways to arrange the array and n choices for each task placement.

**Space Complexity:** O(n^2) due to the recursion stack and memoization table.

```typescript
function minSessions(tasks: number[], sessionTime: number): number {
    const n = tasks.length;
    const memo: Map<string, number> = new Map();

    function backtrack(i: number, sessions: number[]): number {
        if (i === n) {
            return sessions.length;
        }
        
        const key = `${i}-${sessions.join('-')}`;
        if (memo.has(key)) {
            return memo.get(key) as number;
        }
        
        let minSessions = Infinity;
        for (let j = 0; j < sessions.length; j++) {
            if (sessions[j] + tasks[i] <= sessionTime) {
                sessions[j] += tasks[i];
                minSessions = Math.min(minSessions, backtrack(i + 1, sessions));
                sessions[j] -= tasks[i];
            }
        }
        
        sessions.push(tasks[i]);
        minSessions = Math.min(minSessions, backtrack(i + 1, sessions));
        sessions.pop();
        
        memo.set(key, minSessions);
        return minSessions;
    }

    return backtrack(0, []);
}
```

### Dynamic Programming with State Compression

**Intuition:**
We can use dynamic programming to store intermediate results using a bitmask to represent the states of tasks completed. This technique leverages bit manipulation to efficiently represent and process subsets of tasks. The primary idea is to represent each subset of tasks using a bitmask and calculate the minimum number of sessions for each subset.

1. **Subset Representation**: Use bitmasking to represent subsets of tasks.
2. **Dynamic Programming Transition**: For each state's bitmask, try assigning any remaining task to the current work session.
3. **Cost Calculation**: If adding a task to the current session would not exceed `sessionTime`, we calculate the cost for that decision.
4. **Optimization**: Use previously computed results to build up to the final solution.
5. **Base Case**: Empty set (no tasks) requires no sessions.

**Time Complexity:** O(n * 2^n) because there are 2^n subsets of tasks and we iterate over these for up to n iterations per subset.

**Space Complexity:** O(2^n) for storing dp states.

```typescript
function minSessions(tasks: number[], sessionTime: number): number {
    const n = tasks.length;
    const dp = new Array(1 << n).fill(Infinity);
    dp[0] = 0; // Base case: No tasks require 0 sessions

    for (let mask = 0; mask < (1 << n); ++mask) {
        let currentTime = 0;
        
        // Recalculate current time for the tasks represented by `mask`
        for (let i = 0; i < n; ++i) {
            if ((mask & (1 << i)) !== 0) {
                currentTime += tasks[i];
            }
        }
        
        for (let i = 0; i < n; ++i) {
            if ((mask & (1 << i)) === 0) { // Task i is not included in current `mask`
                const newMask = mask | (1 << i);
                if (currentTime + tasks[i] <= sessionTime) {
                    dp[newMask] = Math.min(dp[newMask], dp[mask]); // Same session
                } else {
                    dp[newMask] = Math.min(dp[newMask], dp[mask] + 1); // New session
                }
            }
        }
    }

    return dp[(1 << n) - 1] + 1; // Add 1 to account for the session starting at dp[0]
}
```

In this version of the state compression approach, we effectively transition between states using the dp array, where each state's task completion is indicated by a bitmask. The transition takes into consideration whether including an additional task would exceed the current session's capacity.

