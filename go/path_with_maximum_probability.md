# 1514. [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

## Approach 1: Dijkstra's Algorithm using a Priority Queue

### Solution
go
```go
// Time Complexity: O((n + m) * log n), where n is the number of nodes and m is the number of edges
// Space Complexity: O(n + m)
package main

import (
	"container/heap"
	"math"
)

type Node struct {
	node       int
	probability float64
}

type PriorityQueue []*Node

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
	return pq[i].probability > pq[j].probability // Higher probability comes first
}

func (pq PriorityQueue) Swap(i, j int) {
	pq[i], pq[j] = pq[j], pq[i]
}

func (pq *PriorityQueue) Push(x interface{}) {
	node := x.(*Node)
	*pq = append(*pq, node)
}

func (pq *PriorityQueue) Pop() interface{} {
	old := *pq
	n := len(old)
	node := old[n-1]
	*pq = old[0 : n-1]
	return node
}

func maxProbability(n int, edges [][]int, succProb []float64, start int, end int) float64 {
	graph := make([][]Node, n)
	for i := range graph {
		graph[i] = []Node{}
	}

	for i := 0; i < len(edges); i++ {
		u, v := edges[i][0], edges[i][1]
		prob := succProb[i]
		graph[u] = append(graph[u], Node{v, prob})
		graph[v] = append(graph[v], Node{u, prob}) // Undirected graph
	}

	probabilities := make([]float64, n)
	for i := range probabilities {
		probabilities[i] = 0.0
	}
	probabilities[start] = 1.0

	pq := &PriorityQueue{}
	heap.Init(pq)
	heap.Push(pq, &Node{start, 1.0})

	for pq.Len() > 0 {
		current := heap.Pop(pq).(*Node)
		currNode := current.node
		currProb := current.probability

		if currNode == end {
			return currProb
		}

		if currProb < probabilities[currNode] {
			continue
		}

		for _, neighbor := range graph[currNode] {
			nextNode := neighbor.node
			edgeProb := neighbor.probability
			newProb := currProb * edgeProb
			if newProb > probabilities[nextNode] {
				probabilities[nextNode] = newProb
				heap.Push(pq, &Node{nextNode, newProb})
			}
		}
	}

	return probabilities[end]
}
```

This problem is effectively a modification of the shortest path problem or Dijkstra's algorithm by changing distance sums to probabilities multiplied.

