# 621. [Task Scheduler](https://leetcode.com/problems/task-scheduler/)

## Approach: Greedy Algorithm with Counting

### Solution
```go
// Time Complexity: O(n), where n is the total number of tasks
// Space Complexity: O(1), as the task frequencies are stored in a fixed-size array
package main

import (
	"sort"
)

func leastInterval(tasks []byte, n int) int {
	// Array to store task frequencies
	freq := make([]int, 26)

	// Count the frequency of each task
	for _, task := range tasks {
		freq[task-'A']++
	}

	// Sort the frequencies in descending order
	sort.Ints(freq)

	// Highest frequency
	maxFreq := freq[25]
	// Calculate idle slots
	idleSlots := (maxFreq - 1) * n

	// Deduct the available slots with other task frequencies
	for i := 24; i >= 0 && freq[i] > 0; i-- {
		idleSlots -= min(freq[i], maxFreq-1)
	}

	// If idleSlots are negative, it means no idling is needed
	if idleSlots < 0 {
		idleSlots = 0
	}

	// Total time is tasks + idle slots
	return len(tasks) + idleSlots
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

