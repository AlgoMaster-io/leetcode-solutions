# [Leetcode 1986: Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Approaches:
- [Backtracking](#backtracking)
- [Dynamic Programming with Bitmasking](#dynamic-programming-with-bitmasking)

### Backtracking

#### Intuition:
The problem is similar to the "bin-packing" problem, where we need to efficiently pack items (tasks) into containers (work sessions) of fixed size. A backtracking approach tries to fit each task into a session recursively, keeping track of the sessions used. This is a brute-force approach and mainly serves to understand the problem constraints better.

#### Approach:
1. Use recursive backtracking to try placing each task into any of the current work sessions that have enough remaining time.
2. If a task doesn't fit in existing sessions, start a new session.
3. Track the minimum number of sessions used throughout the process.

```javascript
function minSessions(tasks, sessionTime) {
    const n = tasks.length;
    const sessions = Array(n).fill(sessionTime);

    function backtrack(i) {
        if (i === n) {
            return sessions.reduce((acc, time) => acc + (time !== sessionTime), 0);
        }

        let minSessions = Infinity;

        for (let j = 0; j < n; j++) {
            // Try to fit task[i] in session[j] if it fits
            if (sessions[j] >= tasks[i]) {
                sessions[j] -= tasks[i];
                minSessions = Math.min(minSessions, backtrack(i + 1));
                sessions[j] += tasks[i];
            }
        }

        return minSessions;
    }

    return backtrack(0);
}
```

#### Complexity:
- **Time Complexity**: \(O(n!)\) in the worst case due to trying each task in every session.
- **Space Complexity**: \(O(n)\) due to the recursion stack.

---

### Dynamic Programming with Bitmasking

#### Intuition:
Bitmasking can be used to represent the set of tasks already completed, making it easier to keep track of tasks when using dynamic programming. By considering each possible state as a bitmask, we can fill the DP table based on previously computed states. This approach efficiently computes the minimum sessions required for all tasks.

#### Approach:
1. Use a bitmask `dp` array where `dp[mask]` holds the minimum number of sessions to complete the tasks represented by the bitmask `mask`.
2. For each possible bitmask, check each task to see if adding it can reduce the number of sessions.
3. Transition between states where `dp[mask | (1 << j)]` is filled using states already computed.

```javascript
function minSessions(tasks, sessionTime) {
    const n = tasks.length;
    const allTasks = 1 << n; // Total number of combinations
    const dp = Array(allTasks).fill(Infinity);
    const remaining = Array(allTasks).fill(0);

    // Initialize for empty task set
    dp[0] = 1;
    remaining[0] = sessionTime;

    // Iterate over all states
    for (let mask = 0; mask < allTasks; mask++) {
        if (dp[mask] === Infinity) continue;

        for (let j = 0; j < n; j++) {
            if (!(mask & (1 << j))) {
                let newMask = mask | (1 << j);
                if (remaining[mask] >= tasks[j]) {
                    if (dp[newMask] > dp[mask]) {
                        dp[newMask] = dp[mask];
                        remaining[newMask] = Math.max(remaining[newMask], remaining[mask] - tasks[j]);
                    }
                } else {
                    if (dp[newMask] > dp[mask] + 1) {
                        dp[newMask] = dp[mask] + 1;
                        remaining[newMask] = Math.max(remaining[newMask], sessionTime - tasks[j]);
                    }
                }
            }
        }
    }

    return dp[allTasks - 1];
}
```

#### Complexity:
- **Time Complexity**: \(O(n \times 2^n)\) where \(2^n\) is the number of bitmasks and \(n\) is the task operations within each bitmask.
- **Space Complexity**: \(O(2^n)\) for the `dp` and `remaining` arrays.

