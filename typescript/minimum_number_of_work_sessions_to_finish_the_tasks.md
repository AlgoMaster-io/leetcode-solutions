# [1986. Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Approach 1: Backtracking

### Solution
typescript
```typescript
// Time Complexity: O(n!)
// Space Complexity: O(n)
class Solution {
    private minSessions: number;

    public minSessions(tasks: number[], sessionTime: number): number {
        this.minSessions = tasks.length;
        this.backtrack(tasks, sessionTime, 0, 0, 0, new Array(tasks.length).fill(false));
        return this.minSessions;
    }
    
    private backtrack(tasks: number[], sessionTime: number, currentSessionTime: number, sessionCount: number, taskIndex: number, used: boolean[]): void {
        if (sessionCount >= this.minSessions) {
            return; // Already have a better solution
        }
        
        // Check if all tasks are completed
        let allTasksCompleted = true;
        for (const taskUsed of used) {
            if (!taskUsed) {
                allTasksCompleted = false;
                break;
            }
        }
        if (allTasksCompleted) {
            this.minSessions = Math.min(this.minSessions, sessionCount);
            return;
        }
        
        // Try to place each task into the current session or start a new session
        for (let i = taskIndex; i < tasks.length; i++) {
            if (!used[i]) {
                used[i] = true;
                
                // Check if task fits in the current session
                if (currentSessionTime + tasks[i] <= sessionTime) {
                    this.backtrack(tasks, sessionTime, currentSessionTime + tasks[i], sessionCount, i + 1, used);
                } else {
                    // Start a new session for this task
                    this.backtrack(tasks, sessionTime, tasks[i], sessionCount + 1, i + 1, used);
                }
                
                used[i] = false;
            }
        }
    }
}
```

## Approach 2: Dynamic Programming with Bitmask

### Solution
typescript
```typescript
// Time Complexity: O(n * 2^n)
// Space Complexity: O(2^n)
class Solution {
    public minSessions(tasks: number[], sessionTime: number): number {
        const n = tasks.length;
        const dp = new Array(1 << n).fill(n); // DP table with 2^n states
        const time = new Array(1 << n).fill(0); // Tracks total time for each state
        
        dp[0] = 0; // Base state: no tasks requires no session

        // Iterate over all states
        for (let state = 0; state < (1 << n); state++) {
            if (dp[state] > n) continue; // Skip invalid states

            // Try to add each task to the current state
            for (let i = 0; i < n; i++) {
                if ((state & (1 << i)) === 0) { // Task `i` is not in current state
                    const nextState = state | (1 << i); // Add task `i` to the state
                    // Check if adding task `i` fits in the current session
                    if (time[state] + tasks[i] <= sessionTime) {
                        time[nextState] = time[state] + tasks[i];
                        dp[nextState] = Math.min(dp[nextState], dp[state]);
                    } else {
                        // Start a new session for task `i`
                        time[nextState] = tasks[i];
                        dp[nextState] = Math.min(dp[nextState], dp[state] + 1);
                    }
                }
            }
        }

        return dp[(1 << n) - 1] + 1; // Add one to include the initial zero session
    }
}
```

