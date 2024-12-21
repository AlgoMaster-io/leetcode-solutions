# [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

## Approaches
1. [DFS with Pruning](#dfs-with-pruning)
2. [Dynamic Programming](#dynamic-programming)
3. [Dijkstra's Algorithm (Optimized with Min-Heap)](#dijkstras-algorithm-optimized-with-min-heap)
4. [Bellman-Ford Algorithm](#bellman-ford-algorithm)

### DFS with Pruning

Intuition:
The DFS approach involves exploring all possible paths from the source to the destination with at most K stops. We use a recursive function to traverse the graph, keep track of the current cost, and prune the search path once the cost exceeds the minimum found.

```go
func findCheapestPrice(n int, flights [][]int, src int, dst int, K int) int {
    // Create adjacency list from flights
    graph := make(map[int][][]int)
    for _, flight := range flights {
        u, v, w := flight[0], flight[1], flight[2]
        graph[u] = append(graph[u], []int{v, w})
    }

    minCost := make([][]int, n)
    for i := range minCost {
        minCost[i] = make([]int, K+2)
        for j := range minCost[i] {
            minCost[i][j] = 1<<31 - 1 // Initialize with max int value
        }
    }
    
    var dfs func(node, stops, cost int) int
    dfs = func(node, stops, cost int) int {
        if stops > K+1 || cost >= minCost[node][stops] {
            return 1<<31 - 1 // Return max int value
        }
        
        // Update minimum cost to reach this node with stops
        minCost[node][stops] = cost
        if node == dst {
            return cost
        }
        
        result := 1<<31 - 1
        for _, neighbor := range graph[node] {
            nextNode, price := neighbor[0], neighbor[1]
            result = min(result, dfs(nextNode, stops+1, cost+price))
        }
        return result
    }
    
    answer := dfs(src, 0, 0)
    if answer == 1<<31 - 1 {
        return -1
    }
    return answer
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```
**Time Complexity:** The DFS approach could visit a potentially large number of paths, leading to an exponential time complexity in the worst case.
**Space Complexity:** O(n * (K+1)), due to storing the `minCost` values.

### Dynamic Programming

Intuition:
With dynamic programming, we maintain a 2D dp array where `dp[k][j]` represents the minimum cost to reach city `j` using at most `k` stops. We fill this table iteratively using the provided flight information.

```go
func findCheapestPriceDP(n int, flights [][]int, src int, dst int, K int) int {
    // dp[i][j]: min cost to reach city j using at most i stops
    dp := make([][]int, K+2)
    for i := range dp {
        dp[i] = make([]int, n)
        for j := range dp[i] {
            dp[i][j] = 1<<31 - 1 // Initialize with max int value
        }
    }
    
    // Cost to reach the source is 0 with any stops
    for i := 0; i <= K+1; i++ {
        dp[i][src] = 0
    }
    
    for i := 1; i <= K+1; i++ {
        for _, flight := range flights {
            u, v, w := flight[0], flight[1], flight[2]
            if dp[i-1][u] != 1<<31 - 1 {
                dp[i][v] = min(dp[i][v], dp[i-1][u] + w)
            }
        }
    }
    
    if dp[K+1][dst] == 1<<31 - 1 {
        return -1
    }
    return dp[K+1][dst]
}
```
**Time Complexity:** O((K+1) * E), where E is the number of flights.
**Space Complexity:** O(n * (K+1)), as we store the dp table.

### Dijkstra's Algorithm (Optimized with Min-Heap)

Intuition:
We can optimize our approach using the Dijkstra's Algorithm, which finds the shortest paths from a source node. We use a priority queue (min-heap) to ensure we always extend the shortest path discovered so far.

```go
func findCheapestPriceDijkstra(n int, flights [][]int, src int, dst int, K int) int {
    // Create adjacency list from flights
    graph := make(map[int][][]int)
    for _, flight := range flights {
        u, v, w := flight[0], flight[1], flight[2]
        graph[u] = append(graph[u], []int{v, w})
    }

    // Min-Heap to track minimum cost flights
    heap := &MinHeap{}
    heap.Push(&Node{src, 0, 0}) // {city, cost, stops}
    
    minCost := make([]int, n)
    for i := range minCost {
        minCost[i] = 1<<31 - 1
    }
    minCost[src] = 0

    for heap.Len() > 0 {
        current := heap.Pop().(*Node)
        
        city, stops, cost := current.city, current.stops, current.cost
        if city == dst {
            return cost
        }
        if stops > K {
            continue
        }

        for _, neighbor := range graph[city] {
            nextCity, price := neighbor[0], neighbor[1]
            if cost + price < minCost[nextCity] {
                heap.Push(&Node{nextCity, stops+1, cost + price})
                minCost[nextCity] = cost + price
            }
        }
    }
    return -1
}

type Node struct {
    city, stops, cost int
}

type MinHeap []*Node

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].cost < h[j].cost }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
    *h = append(*h, x.(*Node))
}

func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```
**Time Complexity:** O(E * log(n)), due to the operations on the heap.
**Space Complexity:** O(n + E), due to storage used by the graph and the heap.

### Bellman-Ford Algorithm

Intuition:
The Bellman-Ford algorithm is useful for finding shortest paths in graphs with negative weights, and it can also be used here. We iterate `K+1` times and update costs based on the flights information.

```go
func findCheapestPriceBellmanFord(n int, flights [][]int, src int, dst int, K int) int {
    // Bellman-Ford initializes all distances to a high value
    oldCosts := make([]int, n)
    for i := range oldCosts {
        oldCosts[i] = 1<<31 - 1 // Initialize with max int value
    }
    oldCosts[src] = 0
    
    for i := 0; i <= K; i++ {
        newCosts := append([]int{}, oldCosts...)
        
        for _, flight := range flights {
            u, v, w := flight[0], flight[1], flight[2]
            if oldCosts[u] != 1<<31 - 1 {
                newCosts[v] = min(newCosts[v], oldCosts[u] + w)
            }
        }
        oldCosts = newCosts
    }

    if oldCosts[dst] == 1<<31 - 1 {
        return -1
    }
    return oldCosts[dst]
}
```
**Time Complexity:** O(K * E), due to iterations over the number of stops and flights.
**Space Complexity:** O(n), due to storing costs for each node.

