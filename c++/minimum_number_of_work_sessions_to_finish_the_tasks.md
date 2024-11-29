# [1986. Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Approach 1: Backtracking

### Solution
```cpp
// Time Complexity: O(n!)
// Space Complexity: O(n)
class Solution {
private:
    int minSessions;
    
    void backtrack(vector<int>& tasks, int sessionTime, int currentSessionTime, int sessionCount, int taskIndex, vector<bool>& used) {
        if (sessionCount >= minSessions) {
            return; // Already have a better solution
        }
        
        // Check if all tasks are completed
        bool allTasksCompleted = true;
        for (bool taskUsed : used) {
            if (!taskUsed) {
                allTasksCompleted = false;
                break;
            }
        }
        if (allTasksCompleted) {
            minSessions = std::min(minSessions, sessionCount);
            return;
        }
        
        // Try to place each task into the current session or start a new session
        for (int i = taskIndex; i < tasks.size(); i++) {
            if (!used[i]) {
                used[i] = true;
                
                // Check if task fits in the current session
                if (currentSessionTime + tasks[i] <= sessionTime) {
                    backtrack(tasks, sessionTime, currentSessionTime + tasks[i], sessionCount, i + 1, used);
                } else {
                    // Start a new session for this task
                    backtrack(tasks, sessionTime, tasks[i], sessionCount + 1, i + 1, used);
                }
                
                used[i] = false;
            }
        }
    }
    
public:
    int minSessions(vector<int>& tasks, int sessionTime) {
        minSessions = tasks.size();
        vector<bool> used(tasks.size(), false);
        backtrack(tasks, sessionTime, 0, 0, 0, used);
        return minSessions;
    }
};
```

## Approach 2: Dynamic Programming with Bitmask

### Solution
```cpp
// Time Complexity: O(n * 2^n)
// Space Complexity: O(2^n)
class Solution {
public:
    int minSessions(vector<int>& tasks, int sessionTime) {
        int n = tasks.size();
        vector<int> dp(1 << n, n); // DP table with 2^n states
        vector<int> time(1 << n, 0); // Tracks total time for each state
        
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
                        dp[nextState] = std::min(dp[nextState], dp[state]);
                    } else {
                        // Start a new session for task `i`
                        time[nextState] = tasks[i];
                        dp[nextState] = std::min(dp[nextState], dp[state] + 1);
                    }
                }
            }
        }

        return dp[(1 << n) - 1] + 1; // Add one to include the initial zero session
    }
};
```


