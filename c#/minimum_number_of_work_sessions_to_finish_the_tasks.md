# [1986. Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Approach 1: Backtracking

### Solution
csharp
```csharp
// Time Complexity: O(n!)
// Space Complexity: O(n)
public class Solution {
    private int minSessions;

    public int MinSessions(int[] tasks, int sessionTime) {
        minSessions = tasks.Length;
        Backtrack(tasks, sessionTime, 0, 0, 0, new bool[tasks.Length]);
        return minSessions;
    }

    private void Backtrack(int[] tasks, int sessionTime, int currentSessionTime, int sessionCount, int taskIndex, bool[] used) {
        if (sessionCount >= minSessions) {
            return; // Already have a better solution
        }

        // Check if all tasks are completed
        bool allTasksCompleted = true;
        foreach (bool taskUsed in used) {
            if (!taskUsed) {
                allTasksCompleted = false;
                break;
            }
        }
        if (allTasksCompleted) {
            minSessions = Math.Min(minSessions, sessionCount);
            return;
        }

        // Try to place each task into the current session or start a new session
        for (int i = taskIndex; i < tasks.Length; i++) {
            if (!used[i]) {
                used[i] = true;

                // Check if task fits in the current session
                if (currentSessionTime + tasks[i] <= sessionTime) {
                    Backtrack(tasks, sessionTime, currentSessionTime + tasks[i], sessionCount, i + 1, used);
                } else {
                    // Start a new session for this task
                    Backtrack(tasks, sessionTime, tasks[i], sessionCount + 1, i + 1, used);
                }

                used[i] = false;
            }
        }
    }
}
```

## Approach 2: Dynamic Programming with Bitmask

### Solution
csharp
```csharp
// Time Complexity: O(n * 2^n)
// Space Complexity: O(2^n)
public class Solution {
    public int MinSessions(int[] tasks, int sessionTime) {
        int n = tasks.Length;
        int[] dp = new int[1 << n]; // DP table with 2^n states
        int[] time = new int[1 << n]; // Tracks total time for each state
        Array.Fill(dp, n); // Max sessions is n (worst case each task in new session)

        dp[0] = 0; // Base state: no tasks requires no session

        // Iterate over all states
        for (int state = 0; state < (1 << n); state++) {
            if (dp[state] > n) continue; // Skip invalid states

            // Try to add each task to the current state
            for (int i = 0; i < n; i++) {
                if ((state & (1 << i)) == 0) { // Task `i` is not in current state
                    int nextState = state | (1 << i); // Add task `i` to the state
                    // Check if adding task `i` fits in the current session
                    if (time[state] + tasks[i] <= sessionTime) {
                        time[nextState] = time[state] + tasks[i];
                        dp[nextState] = Math.Min(dp[nextState], dp[state]);
                    } else {
                        // Start a new session for task `i`
                        time[nextState] = tasks[i];
                        dp[nextState] = Math.Min(dp[nextState], dp[state] + 1);
                    }
                }
            }
        }

        return dp[(1 << n) - 1] + 1; // Add one to include the initial zero session
    }
}
```

