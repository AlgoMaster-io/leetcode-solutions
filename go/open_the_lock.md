# 752. [Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Approach: Breadth-First Search (BFS)

### Solution
go
```go
// Time Complexity: O(10^4 + d), where d is the size of the deadends
// Space Complexity: O(10^4 + d), for the visited set and the queue
package main

import (
	"fmt"
)

func openLock(deadends []string, target string) int {
	deadSet := make(map[string]bool)
	for _, dead := range deadends {
		deadSet[dead] = true
	}

	if deadSet["0000"] {
		return -1 // Cannot start from the initial position
	}

	queue := []string{"0000"}
	visited := make(map[string]bool)
	visited["0000"] = true

	moves := 0

	// Perform BFS
	for len(queue) > 0 {
		size := len(queue)

		for i := 0; i < size; i++ {
			current := queue[0]
			queue = queue[1:]

			if current == target {
				return moves // Target reached
			}

			// Generate all possible next combinations
			for j := 0; j < 4; j++ {
				c := current[j]

				// Turn the wheel one step forward
				forward := current[:j] + string((c-'0'+1)%10+'0') + current[j+1:]
				if !deadSet[forward] && !visited[forward] {
					queue = append(queue, forward)
					visited[forward] = true
				}

				// Turn the wheel one step backward
				backward := current[:j] + string((c-'0'+9)%10+'0') + current[j+1:]
				if !deadSet[backward] && !visited[backward] {
					queue = append(queue, backward)
					visited[backward] = true
				}
			}
		}

		moves++
	}

	return -1 // Target is unreachable
}

func main() {
	deadends := []string{"0201", "0101", "0102", "1212", "2002"}
	target := "0202"
	fmt.Println(openLock(deadends, target))
}
```

