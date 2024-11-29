# 57. [Insert Interval](https://leetcode.com/problems/insert-interval/)

## Approach: Iteration and Merge

### Solution
```go
// Time Complexity: O(n), where n is the number of intervals
// Space Complexity: O(n), for the result slice
package main

func insert(intervals [][]int, newInterval []int) [][]int {
	result := [][]int{}
	i, n := 0, len(intervals)

	// Step 1: Add all intervals that come before the new interval
	for i < n && intervals[i][1] < newInterval[0] {
		result = append(result, intervals[i])
		i++
	}

	// Step 2: Merge overlapping intervals with the new interval
	for i < n && intervals[i][0] <= newInterval[1] {
		if newInterval[0] > intervals[i][0] {
			newInterval[0] = intervals[i][0]
		}
		if newInterval[1] < intervals[i][1] {
			newInterval[1] = intervals[i][1]
		}
		i++
	}
	result = append(result, newInterval) // Add the merged interval

	// Step 3: Add all intervals that come after the new interval
	for i < n {
		result = append(result, intervals[i])
		i++
	}

	return result
}
```

