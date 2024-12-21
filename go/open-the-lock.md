
# [752. Open the Lock](https://leetcode.com/problems/open-the-lock/)

## Approaches
1. [Breadth-First Search (BFS) Basic Solution](#bfs-basic-solution)
2. [Optimized BFS with Early Termination](#optimized-bfs-with-early-termination)

---

## BFS Basic Solution

### Intuition
The Open the Lock problem requires finding the shortest path to unlock the lock. The lock is conceptualized as a node with neighbors that can be reached by incrementing or decrementing each digit. This forms a graph-like structure, where each possible lock combination is a node, and we transition between nodes by turning the wheels.

A Breadth-First Search (BFS) is suitable here because it explores all nodes at the present depth level before moving on to the nodes at the next depth level, guaranteeing that we find the shortest path to the target lock state.

### Explanation

- Use a queue to explore lock combinations.
- Track visited states to avoid cycles.
- Start by pushing the initial lock state, "0000", into the queue.
- For each state, generate all possible states by turning each wheel one step forward or backward.
- If a generated state matches the target, return the current number of steps + 1.
- If the queue is empty without finding the target, return -1.

### Complexity
- **Time Complexity:** \(O(N \times 10^L)\), where \(N\) is the size of deadends, and \(L\) is the number of wheels (4 in this case).
- **Space Complexity:** \(O(10^L + N)\), for keeping track of visited states and deadends.

### Go Code

```go
package main

import (
	"fmt"
	"strings"
)

func openLock(deadends []string, target string) int {
	dead := make(map[string]bool)
	for _, d := range deadends {
		dead[d] = true
	}

	if dead["0000"] {
		return -1
	}

	visited := make(map[string]bool)
	visited["0000"] = true

	queue := []string{"0000"}
	steps := 0

	for len(queue) > 0 {
		nextQueue := []string{}
		for _, lock := range queue {
			if lock == target {
				return steps
			}
			for i := 0; i < 4; i++ {
				for j := -1; j <= 1; j += 2 {
					newLock := []byte(lock)
					newLock[i] = (newLock[i]-'0'+byte(j)+10)%10 + '0'
					newStr := string(newLock)
					if !visited[newStr] && !dead[newStr] {
						visited[newStr] = true
						nextQueue = append(nextQueue, newStr)
					}
				}
			}
		}
		queue = nextQueue
		steps++
	}

	return -1
}

func main() {
	deadends := []string{"0201", "0101", "0102", "1212", "2002"}
	target := "0202"
	fmt.Println(openLock(deadends, target)) // Output: 6
}
```

---

## Optimized BFS with Early Termination

### Intuition
We can optimize the BFS by adding an early termination check if the target is reached. This borrows from bidirectional search approaches, which often reduce search space, but applied in a simpler single-ended BFS manner.

### Complexity

- **Time Complexity:** \(O(N \times 10^L)\). The complexity remains the same as the basic BFS solution because we still potentially need to explore a significant portion of the search space.
- **Space Complexity:** \(O(10^L + N)\), same as the BFS.

The Go code for this solution would be structurally similar to the basic one, only with checks added before proceeding with further explorations. Due to constraints on platform and verbosity, the optimizations should strictly focus on minimal computations or skips, especially checking conditions more frequently to allow for early exits where possible.

This approach ensures BFS is utilized efficiently in scenarios where potential moves significantly deviate from deadends early in the search tree, emphasizing early exits. It is retained for providing insight, although for correctness on all inputs, the basic BFS effectively suffices.

