# [1986. Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Approaches
- [Approach 1: Brute Force Using Recursion](#approach-1)
- [Approach 2: Dynamic Programming with Bitmasking](#approach-2)

### Approach 1: Brute Force Using Recursion

**Intuition**:  
This approach involves exploring all possible ways to assign tasks to work sessions to identify the minimal number of sessions required. We recursively try placing each task in a session and backtrack if the current assignment doesn't lead to a viable solution.

#### Steps:
1. **Recursive DFS Function**: Write a recursive function `dfs(pos, sessions)` where `pos` is the index of the current task we're considering, and `sessions` is a vector containing the remaining time in each session.
2. **Base Condition**: If `pos` reaches the number of tasks, compare against a global minimum session count.
3. **Try Placing the Task**: For each session, try placing the task if it fits within the remaining time of that session.
4. **Create a New Session**: If none of the sessions can accommodate the task, create a new session and attempt again.

```cpp
#include <vector>
#include <algorithm>
#include <numeric>

class Solution {
public:
    int minSessions(std::vector<int>& tasks, int sessionTime) {
        int n = tasks.size();
        int ans = n;  // Maximum sessions needed will be number of tasks
        std::vector<int> sessions;
        
        // Helper recursive function
        std::function<void(int)> dfs = [&](int pos) {
            if (pos == n) {
                ans = std::min(ans, static_cast<int>(sessions.size()));
                return;
            }
            
            for (int i = 0; i < sessions.size(); ++i) {
                if (sessions[i] >= tasks[pos]) {
                    sessions[i] -= tasks[pos];
                    dfs(pos + 1);
                    sessions[i] += tasks[pos];
                }
            }
            
            sessions.push_back(sessionTime - tasks[pos]);
            dfs(pos + 1);
            sessions.pop_back();
        };
        
        std::sort(tasks.begin(), tasks.end(), std::greater<int>());  // Sort tasks by decreasing time
        dfs(0);  // Start DFS from the first task
        return ans;
    }
};
```

- **Time Complexity**: O(n!), where n is the number of tasks.
- **Space Complexity**: O(n), depth of recursion can go up to the number of tasks.

### Approach 2: Dynamic Programming with Bitmasking

**Intuition**:  
This technique uses dynamic programming combined with bitmasking to represent the state of task completion efficiently. By using a memoized DP table, overlapping subproblems are cached for retrieval for optimized computation.

#### Steps:
1. **State Representation**: Use a bitmask to store whether a task is completed, leading to 2^n states.
2. **DP Table Initialization**: dp[mask] represents the minimum sessions needed to complete tasks represented by `mask`.
3. **Bitmask Iteration**: For each bitmask, determine which tasks can fill a session, and transition to the next state.
4. **Subset Enumeration**: Use subset enumeration to efficiently calculate valid sessions.

```cpp
#include <vector>
#include <cstring>

class Solution {
public:
    int minSessions(std::vector<int>& tasks, int sessionTime) {
        int n = tasks.size();
        int dp[1 << n];  // DP table to store minimum sessions for each bitmask
        std::fill(dp, dp + (1 << n), n);  // Initialize with maximal possible sessions
        
        dp[0] = 0;  // Base case: no tasks need zero sessions
        
        for (int mask = 0; mask < (1 << n); ++mask) {
            int sum = 0;
            for (int i = 0; i < n; ++i) {
                if (mask & (1 << i)) {  // if the ith task is included in the mask
                    sum += tasks[i];
                }
            }
            
            if (sum <= sessionTime) {
                for (int subset = mask; subset; subset = (subset - 1) & mask) {  // Iterate over subsets
                    if (sum - totalInMask(subset, tasks) <= sessionTime) {
                        dp[mask] = std::min(dp[mask], dp[mask ^ subset] + 1);
                    }
                }
            }
        }
        
        return dp[(1 << n) - 1];  // Minimum sessions needed for all tasks
    }
    
private:
    int totalInMask(int mask, const std::vector<int>& tasks) {
        int total = 0;
        for (int i = 0; mask; ++i, mask >>= 1) {
            if (mask & 1) {
                total += tasks[i];
            }
        }
        return total;
    }
};
```

- **Time Complexity**: O(N * 2^N), iterating over the state combinations.
- **Space Complexity**: O(2^N), as we are maintaining a DP array for all subsets.

In conclusion, the first approach leverages basic recursive backtracking to explore possible task-to-session mappings, while the second approach uses dynamic programming with bitmasking for efficient state management. The choice between them should be guided by the size of `n` (number of tasks) and the complexity you can afford.

