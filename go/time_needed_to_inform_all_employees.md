# 1376. [Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)

## Approach: Depth-First Search (DFS)

### Solution
go
```go
// Time Complexity: O(n), where n is the number of employees
// Space Complexity: O(n), for the adjacency list and recursion stack
package main

import "fmt"

func numOfMinutes(n, headID int, manager, informTime []int) int {
	// Step 1: Build an adjacency list for the employee-manager hierarchy
	adjList := make([][]int, n)
	for i := range adjList {
		adjList[i] = []int{}
	}

	for i := 0; i < n; i++ {
		if manager[i] != -1 {
			adjList[manager[i]] = append(adjList[manager[i]], i)
		}
	}

	// Step 2: Perform DFS to calculate the total time to inform all employees
	return dfs(headID, adjList, informTime)
}

func dfs(current int, adjList [][]int, informTime []int) int {
	maxTime := 0

	// Recur for all subordinates of the current employee
	for _, subordinate := range adjList[current] {
		maxTime = max(maxTime, dfs(subordinate, adjList, informTime))
	}

	// Total time is the current employee's inform time + max time of subordinates
	return informTime[current] + maxTime
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	n := 6
	headID := 2
	manager := []int{2, 2, -1, 2, 2, 2}
	informTime := []int{0, 0, 1, 0, 0, 0}

	fmt.Println(numOfMinutes(n, headID, manager, informTime)) // Output: 1
}
```

