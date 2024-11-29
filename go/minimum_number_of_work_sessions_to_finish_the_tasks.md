# [1986. Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Approach 1: Backtracking

### Solution
go
```go
// Time Complexity: O(n!)
// Space Complexity: O(n)
package main

import (
	"math"
)

func minSessions(tasks []int, sessionTime int) int {
	minSessions := len(tasks)
	used := make([]bool, len(tasks))
	backtrack(tasks, sessionTime, 0, 0, 0, used, &minSessions)
	return minSessions
}

func backtrack(tasks []int, sessionTime, currentSessionTime, sessionCount, taskIndex int, used []bool, minSessions *int) {
	if sessionCount >= *minSessions {
		return // Already have a better solution
	}

	// Check if all tasks are completed
	allTasksCompleted := true
	for _, taskUsed := range used {
		if !taskUsed {
			allTasksCompleted = false
			break
		}
	}
	if allTasksCompleted {
		*minSessions = int(math.Min(float64(*minSessions), float64(sessionCount)))
		return
	}

	// Try to place each task into the current session or start a new session
	for i := taskIndex; i < len(tasks); i++ {
		if !used[i] {
			used[i] = true

			// Check if task fits in the current session
			if currentSessionTime+tasks[i] <= sessionTime {
				backtrack(tasks, sessionTime, currentSessionTime+tasks[i], sessionCount, i+1, used, minSessions)
			} else {
				// Start a new session for this task
				backtrack(tasks, sessionTime, tasks[i], sessionCount+1, i+1, used, minSessions)
			}

			used[i] = false
		}
	}
}
```

## Approach 2: Dynamic Programming with Bitmask

### Solution
go
```go
// Time Complexity: O(n * 2^n)
// Space Complexity: O(2^n)
package main

import (
	"math/bits"
)

func minSessions(tasks []int, sessionTime int) int {
	n := len(tasks)
	dp := make([]int, 1<<n)    // DP table with 2^n states
	time := make([]int, 1<<n)  // Tracks total time for each state
	for i := range dp {
		dp[i] = n // Max sessions is n (worst case each task in new session)
	}
	dp[0] = 0 // Base state: no tasks requires no session

	// Iterate over all states
	for state := 0; state < (1 << n); state++ {
		if dp[state] > n {
			continue // Skip invalid states
		}

		// Try to add each task to the current state
		for i := 0; i < n; i++ {
			if (state & (1 << i)) == 0 { // Task `i` is not in current state
				nextState := state | (1 << i) // Add task `i` to the state
				// Check if adding task `i` fits in the current session
				if time[state]+tasks[i] <= sessionTime {
					time[nextState] = time[state] + tasks[i]
					dp[nextState] = int(math.Min(float64(dp[nextState]), float64(dp[state])))
				} else {
					// Start a new session for task `i`
					time[nextState] = tasks[i]
					dp[nextState] = int(math.Min(float64(dp[nextState]), float64(dp[state]+1)))
				}
			}
		}
	}

	return dp[(1<<n)-1] + 1 // Add one to include the initial zero session
}
```

