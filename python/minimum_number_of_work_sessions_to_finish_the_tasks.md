# [1986. Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Approach 1: Backtracking

### Solution
python
```python
# Time Complexity: O(n!)
# Space Complexity: O(n)
class Solution:
    def minSessions(self, tasks, sessionTime):
        self.min_sessions = len(tasks)
        
        def backtrack(tasks, session_time, current_session_time, session_count, task_index, used):
            if session_count >= self.min_sessions:
                return  # Already have a better solution
            
            # Check if all tasks are completed
            all_tasks_completed = all(used)
            if all_tasks_completed:
                self.min_sessions = min(self.min_sessions, session_count)
                return
            
            # Try to place each task into the current session or start a new session
            for i in range(task_index, len(tasks)):
                if not used[i]:
                    used[i] = True
                    
                    # Check if task fits in the current session
                    if current_session_time + tasks[i] <= session_time:
                        backtrack(tasks, session_time, current_session_time + tasks[i], session_count, i + 1, used)
                    else:
                        # Start a new session for this task
                        backtrack(tasks, session_time, tasks[i], session_count + 1, i + 1, used)
                    
                    used[i] = False
        
        used = [False] * len(tasks)
        backtrack(tasks, sessionTime, 0, 0, 0, used)
        return self.min_sessions
```

## Approach 2: Dynamic Programming with Bitmask

### Solution
python
```python
# Time Complexity: O(n * 2^n)
# Space Complexity: O(2^n)
class Solution:
    def minSessions(self, tasks, sessionTime):
        n = len(tasks)
        max_state = 1 << n
        dp = [n] * max_state  # Max sessions is n (worst case each task in new session)
        time = [0] * max_state  # Tracks total time for each state
        
        dp[0] = 0  # Base state: no tasks requires no session

        # Iterate over all states
        for state in range(max_state):
            if dp[state] > n:
                continue  # Skip invalid states

            # Try to add each task to the current state
            for i in range(n):
                if (state & (1 << i)) == 0:  # Task `i` is not in current state
                    next_state = state | (1 << i)  # Add task `i` to the state
                    # Check if adding task `i` fits in the current session
                    if time[state] + tasks[i] <= sessionTime:
                        time[next_state] = time[state] + tasks[i]
                        dp[next_state] = min(dp[next_state], dp[state])
                    else:
                        # Start a new session for task `i`
                        time[next_state] = tasks[i]
                        dp[next_state] = min(dp[next_state], dp[state] + 1)

        return dp[max_state - 1] + 1  # Add one to include the initial zero session
```

