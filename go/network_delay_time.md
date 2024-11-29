# 743. [Network Delay Time](https://leetcode.com/problems/network-delay-time/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
```go
// Time Complexity: O((V + E) log V)
// Space Complexity: O(V + E)
import (
	"container/heap"
	"math"
)

type Edge struct {
	target, time int
}

type PriorityQueueItem struct {
	node, time int
}

type PriorityQueue []PriorityQueueItem

// Implement the heap interface for PriorityQueue
func (pq PriorityQueue) Len() int           { return len(pq) }
func (pq PriorityQueue) Less(i, j int) bool { return pq[i].time < pq[j].time }
func (pq PriorityQueue) Swap(i, j int)      { pq[i], pq[j] = pq[j], pq[i] }

func (pq *PriorityQueue) Push(x interface{}) {
	*pq = append(*pq, x.(PriorityQueueItem))
}

func (pq *PriorityQueue) Pop() interface{} {
	old := *pq
	n := len(old)
	item := old[n-1]
	*pq = old[:n-1]
	return item
}

func networkDelayTime(times [][]int, n int, k int) int {
	graph := make(map[int][]Edge)
	for _, time := range times {
		graph[time[0]] = append(graph[time[0]], Edge{time[1], time[2]})
	}

	pq := &PriorityQueue{}
	heap.Init(pq)
	heap.Push(pq, PriorityQueueItem{k, 0})

	minTimeToReach := make(map[int]int)

	for pq.Len() > 0 {
		item := heap.Pop(pq).(PriorityQueueItem)
		currentNode, currentTime := item.node, item.time

		if _, exists := minTimeToReach[currentNode]; exists {
			continue
		}

		minTimeToReach[currentNode] = currentTime
		for _, edge := range graph[currentNode] {
			if _, visited := minTimeToReach[edge.target]; !visited {
				heap.Push(pq, PriorityQueueItem{edge.target, currentTime + edge.time})
			}
		}
	}

	if len(minTimeToReach) != n {
		return -1
	}

	maxTime := 0
	for _, time := range minTimeToReach {
		if time > maxTime {
			maxTime = time
		}
	}

	return maxTime
}
```

## Approach 2: Bellman-Ford Algorithm

### Solution
```go
// Time Complexity: O(V * E)
// Space Complexity: O(V)
import "math"

func networkDelayTime(times [][]int, n int, k int) int {
	// Initialize distances with a large value
	dist := make([]int, n+1)
	for i := range dist {
		dist[i] = math.MaxInt32
	}
	dist[k] = 0

	// Relaxation process for V-1 times
	for i := 1; i < n; i++ {
		for _, edge := range times {
			u, v, w := edge[0], edge[1], edge[2]
			if dist[u] != math.MaxInt32 && dist[u]+w < dist[v] {
				dist[v] = dist[u] + w
			}
		}
	}

	maxTime := 0
	for i := 1; i <= n; i++ {
		if dist[i] == math.MaxInt32 {
			return -1
		}
		if dist[i] > maxTime {
			maxTime = dist[i]
		}
	}

	return maxTime
}
```

## Approach 3: Dynamic Programming with Floyd-Warshall Algorithm

### Solution
```go
// Time Complexity: O(V^3)
// Space Complexity: O(V^2)
import "math"

func networkDelayTime(times [][]int, n int, k int) int {
	dp := make([][]int, n+1)
	for i := range dp {
		dp[i] = make([]int, n+1)
		for j := range dp[i] {
			if i == j {
				dp[i][j] = 0
			} else {
				dp[i][j] = math.MaxInt32 / 2 // We use MaxInt32 / 2 to prevent overflow during calculations
			}
		}
	}

	for _, time := range times {
		dp[time[0]][time[1]] = time[2]
	}

	// Floyd-Warshall algorithm
	for t := 1; t <= n; t++ {
		for u := 1; u <= n; u++ {
			for v := 1; v <= n; v++ {
				if dp[u][t] != math.MaxInt32 && dp[t][v] != math.MaxInt32 {
					if dp[u][v] > dp[u][t]+dp[t][v] {
						dp[u][v] = dp[u][t] + dp[t][v]
					}
				}
			}
		}
	}

	maxTime := 0
	for i := 1; i <= n; i++ {
		if dp[k][i] == math.MaxInt32 {
			return -1
		}
		if dp[k][i] > maxTime {
			maxTime = dp[k][i]
		}
	}

	return maxTime
}
```

