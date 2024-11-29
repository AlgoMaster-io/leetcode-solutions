# 1976. [Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

## Approach 1: Dijkstra's Algorithm with Modification

### Solution
```go
// Time Complexity: O(E log V) where E is the number of edges and V is the number of vertices
// Space Complexity: O(V)
import (
	"container/heap"
	"math"
)

func countPaths(n int, roads [][]int) int {
	const MOD = 1_000_000_007
	graph := make([][]int, n)
	for i := 0; i < n; i++ {
		graph[i] = nil
	}

	// Construct the graph
	for _, road := range roads {
		u, v, time := road[0], road[1], road[2]
		graph[u] = append(graph[u], []int{v, time})
		graph[v] = append(graph[v], []int{u, time})
	}

	minDist := make([]int64, n)
	ways := make([]int, n)
	for i := range minDist {
		minDist[i] = math.MaxInt64
	}
	ways[0] = 1
	minDist[0] = 0

	pq := &PriorityQueue{}
	heap.Init(pq)
	heap.Push(pq, &Item{distance: 0, node: 0})

	for pq.Len() > 0 {
		current := heap.Pop(pq).(*Item)
		currentDist := current.distance
		u := current.node

		if currentDist > minDist[u] {
			continue
		}

		// Traverse neighbors
		for _, neighbor := range graph[u] {
			v := neighbor[0]
			time := int64(neighbor[1])

			// Relaxation
			if minDist[u]+time < minDist[v] {
				minDist[v] = minDist[u] + time
				ways[v] = ways[u]
				heap.Push(pq, &Item{distance: minDist[v], node: v})
			} else if minDist[u]+time == minDist[v] {
				ways[v] = (ways[v] + ways[u]) % MOD
			}
		}
	}

	return ways[n-1]
}

type Item struct {
	distance int64
	node     int
}

type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
	return pq[i].distance < pq[j].distance
}

func (pq PriorityQueue) Swap(i, j int) {
	pq[i], pq[j] = pq[j], pq[i]
}

func (pq *PriorityQueue) Push(x interface{}) {
	item := x.(*Item)
	*pq = append(*pq, item)
}

func (pq *PriorityQueue) Pop() interface{} {
	old := *pq
	n := len(old)
	item := old[n-1]
	*pq = old[0 : n-1]
	return item
}
```

This solution efficiently finds the number of shortest paths from node 0 to node n-1 using a modified version of Dijkstra's algorithm that keeps track of the number of ways to arrive at each node. It uses a priority queue to explore the shortest paths and maintains an array to store the number of ways to reach each node at the shortest distance.

