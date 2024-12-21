## [Leetcode 1976: Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

### Approaches:
1. [Dijkstra's Algorithm with Dynamic Programming](#dijkstra-dp)

---

### 1. Dijkstra's Algorithm with Dynamic Programming

Dijkstra's algorithm is a well-known method for finding shortest paths from a source node to all other nodes in a graph with non-negative edge weights. In this problem, we need to find not only the shortest paths but also count the number of distinct paths that reach the destination node with the shortest length.

#### Intuition:
- Start by interpreting the problem as a shortest-path problem in a graph, where nodes represent intersections, and edges represent roads with associated travel time.
- Use Dijkstra's algorithm to find the shortest path from the source node (0) to the destination node (`n-1`).
- Maintain additional data structures to keep track of the number of ways to arrive at each node using paths of the shortest length.
- At each step, update the number of ways to reach a node only if we find an equally shortest path.

#### Detailed Steps:
1. Initialize a priority queue to simulate a min-heap, helping us efficiently get the node with the shortest current known path.
2. Use an array `dist` where `dist[i]` holds the shortest distance from node 0 to node i.
3. Use an array `ways` where `ways[i]` holds the number of ways to reach node i using paths of length `dist[i]`.
4. Initialize the `ways[0]` as 1 because there's exactly one way to be at the start.
5. For every node processed, check its neighbors to update their `dist` and `ways` appropriately. If a shorter path to a neighbor is found, update its distance and reset the number of ways; if an equally shortest path is found, just increment the number of ways.

#### Code:
```go
package main

import (
	"container/heap"
	"math"
)

type Edge struct {
	node, time int
}

type MinHeap []Edge

func (h MinHeap) Len() int { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].time < h[j].time }
func (h MinHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(Edge)) }
func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	item := old[n-1]
	*h = old[0 : n-1]
	return item
}

func countPaths(n int, roads [][]int) int {
	const MOD = 1000000007
	
	// Step 1: Build the graph adjacency list
	graph := make([][]Edge, n)
	for _, road := range roads {
		u, v, time := road[0], road[1], road[2]
		graph[u] = append(graph[u], Edge{v, time})
		graph[v] = append(graph[v], Edge{u, time})
	}
	
	// Initialize distances and ways arrays
	dist := make([]int, n)
	ways := make([]int, n)
	for i := 0; i < n; i++ {
		dist[i] = math.MaxInt64
	}
	dist[0] = 0
	ways[0] = 1
	
	// Min-heap for Dijkstra's algorithm
	minHeap := &MinHeap{}
	heap.Init(minHeap)
	heap.Push(minHeap, Edge{0, 0})
	
	for minHeap.Len() > 0 {
		cur := heap.Pop(minHeap).(Edge)
		curNode, curTime := cur.node, cur.time
		
		// Process each neighbor
		for _, neighbor := range graph[curNode] {
			nextNode, travelTime := neighbor.node, neighbor.time
			newDist := curTime + travelTime
			
			if newDist < dist[nextNode] {
				dist[nextNode] = newDist
				ways[nextNode] = ways[curNode]
				heap.Push(minHeap, Edge{nextNode, newDist})
			} else if newDist == dist[nextNode] {
				ways[nextNode] = (ways[nextNode] + ways[curNode]) % MOD
			}
		}
	}
	
	// Return the number of ways to arrive at the destination node n-1
	return ways[n-1]
}
```

#### Complexity Analysis:
- **Time Complexity:** O((V + E) * log V), where V is the number of nodes and E is the number of edges. The log V factor comes from the use of the priority queue.
- **Space Complexity:** O(V + E) needed for the adjacency list representation of the graph, distances, and ways arrays.

This approach efficiently combines Dijkstra's algorithm with dynamic programming to calculate both the shortest path lengths and the count of such paths to the destination node.

