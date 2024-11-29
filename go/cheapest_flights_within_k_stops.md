# 787. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Approach 1: DFS with Pruning

### Solution
go
```go
// Time Complexity: O(n^k) in the worst case
// Space Complexity: O(n)
import "math"

func findCheapestPrice(n int, flights [][]int, src int, dst int, K int) int {
    result := math.MaxInt32

    // Create adjacency map for graph edges
    graph := make(map[int]map[int]int)
    for _, flight := range flights {
        if _, exists := graph[flight[0]]; !exists {
            graph[flight[0]] = make(map[int]int)
        }
        graph[flight[0]][flight[1]] = flight[2]
    }

    // Perform DFS
    var dfs func(node, stops, cost int)
    dfs = func(node, stops, cost int) {
        if node == dst {
            result = cost
            return
        }
        
        if stops == 0 || cost >= result {
            return // No more stops available or cost exceeds current best
        }

        if neighbors, exists := graph[node]; exists {
            for neighbor, price := range neighbors {
                dfs(neighbor, stops-1, cost+price)
            }
        }
    }

    dfs(src, K+1, 0) // K + 1 because we count steps, not edges

    if result == math.MaxInt32 {
        return -1
    }
    return result
}
```

## Approach 2: Bellman-Ford Algorithm

### Solution
go
```go
// Time Complexity: O(K * E), where E is the number of edges
// Space Complexity: O(n)
import "math"

func findCheapestPrice(n int, flights [][]int, src int, dst int, K int) int {
    prices := make([]int, n)
    for i := range prices {
        prices[i] = math.MaxInt32
    }
    prices[src] = 0

    // Relax the edges up to K+1 times
    for i := 0; i <= K; i++ {
        tempPrices := append([]int(nil), prices...)
        for _, flight := range flights {
            u, v, price := flight[0], flight[1], flight[2]
            if prices[u] != math.MaxInt32 && prices[u]+price < tempPrices[v] {
                tempPrices[v] = prices[u] + price
            }
        }
        prices = tempPrices
    }

    if prices[dst] == math.MaxInt32 {
        return -1
    }
    return prices[dst]
}
```

## Approach 3: Dijkstra's Algorithm with Priority Queue

### Solution
go
```go
// Time Complexity: O(E + VlogV), where E is the number of edges and V is the number of vertices
// Space Complexity: O(n)
import (
    "container/heap"
    "math"
)

type Item struct {
    cost, node, stopsRemaining int
}

type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
    return pq[i].cost < pq[j].cost
}

func (pq PriorityQueue) Swap(i, j int) { pq[i], pq[j] = pq[j], pq[i] }

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

func findCheapestPrice(n int, flights [][]int, src int, dst int, K int) int {
    graph := make(map[int][][]int)
    for _, flight := range flights {
        graph[flight[0]] = append(graph[flight[0]], []int{flight[1], flight[2]})
    }
    
    pq := &PriorityQueue{}
    heap.Init(pq)
    heap.Push(pq, &Item{cost: 0, node: src, stopsRemaining: K + 1})

    for pq.Len() > 0 {
        current := heap.Pop(pq).(*Item)
        cost, node, stopsRemaining := current.cost, current.node, current.stopsRemaining

        if node == dst {
            return cost
        }

        if stopsRemaining > 0 {
            if neighbors, exists := graph[node]; exists {
                for _, neighbor := range neighbors {
                    nextNode, priceToNext := neighbor[0], neighbor[1]
                    heap.Push(pq, &Item{cost: cost + priceToNext, node: nextNode, stopsRemaining: stopsRemaining - 1})
                }
            }
        }
    }

    return -1 // No valid path found
}
```

