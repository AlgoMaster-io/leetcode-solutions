## Problem: [Leetcode 743: Network Delay Time](https://leetcode.com/problems/network-delay-time/)

## Approaches
- [Approach 1: Dijkstra's Algorithm using Min-Heap](#approach-1-dijkstras-algorithm-using-min-heap)
- [Approach 2: Bellman-Ford Algorithm](#approach-2-bellman-ford-algorithm)

### Approach 1: Dijkstra's Algorithm using Min-Heap

**Intuition:**

The problem can be visualized as finding the shortest path from a single source node (the starting node K) to all other nodes in a directed and weighted graph. Dijkstra's algorithm is well-suited for this task when all edge weights are non-negative.

**Explanation:**

1. **Graph Representation:**
    - Represent the graph using an adjacency list. This will help in efficiently retrieving all the neighboring nodes and their respective travel times.

2. **Priority Queue (Min-Heap):**
    - Use a min-heap to keep track of the nodes to be processed in terms of their accumulated travel time (distance).
    - Start by adding the source node `K` with a time of 0 to begin the traversal.

3. **Relaxation:**
    - Pop the node with the smallest time from the heap.
    - Relax all adjacent edges; if a shorter path is found, update the path length and push the neighboring node into the heap with the new path length.

4. **Completeness Check:**
    - If there are any nodes that are unreachable (i.e., their path length remains infinite), return -1.
    - Otherwise, return the maximum path length which represents the time it will take for all nodes to receive the signal.

**Code:**

```go
import (
    "container/heap"
    "math"
)

// This type will implement heap.Interface for the priority queue
type MinHeap [][2]int

func (h MinHeap) Len() int            { return len(h) }
func (h MinHeap) Less(i, j int) bool  { return h[i][1] < h[j][1] }
func (h MinHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.([2]int)) }
func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func networkDelayTime(times [][]int, n int, k int) int {
    // Step 1: Create Graph as adjacency list
    graph := make(map[int][][2]int)
    for _, time := range times {
        u, v, w := time[0], time[1], time[2]
        graph[u] = append(graph[u], [2]int{v, w})
    }

    // Step 2: Initialize distances with Infinity, zero for the start node
    dist := make([]int, n+1)
    for i := 1; i <= n; i++ {
        dist[i] = math.MaxInt32
    }
    dist[k] = 0

    // Step 3: Min-Heap and start Dijkstra's Algorithm from K
    minHeap := &MinHeap{}
    heap.Push(minHeap, [2]int{k, 0})

    for len(*minHeap) > 0 {
        current := heap.Pop(minHeap).([2]int)
        node, d := current[0], current[1]

        if d > dist[node] {
            continue
        }

        for _, edge := range graph[node] {
            neighbor, weight := edge[0], edge[1]
            if d+weight < dist[neighbor] {
                dist[neighbor] = d + weight
                heap.Push(minHeap, [2]int{neighbor, dist[neighbor]})
            }
        }
    }

    // Step 4: Find the maximum time in distances
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

**Complexity Analysis:**

- **Time Complexity:** \(O(E \log V)\), where \(E\) is the number of edges and \(V\) is the number of vertices. The log factor comes from the operations on the priority queue.
- **Space Complexity:** \(O(V + E)\) for storing the graph and the priority queue.

### Approach 2: Bellman-Ford Algorithm

**Intuition:**

The Bellman-Ford algorithm solves the single-source shortest paths problem from a starting node to all other nodes and can handle graphs with negative weight edges (although this problem only contains non-negative edges).

**Explanation:**

1. **Initialization:**
    - Set the distance to the source node \(K\) to \(0\) and all other nodes to infinity.

2. **Edge Relaxation:**
    - Perform \(n-1\) iterations, where in each iteration, relax all the edges. An edge relaxation updates the path length for destination vertex if a shorter path is found.

3. **Final Check:**
    - If after \(n-1\) iterations, any edge can be further relaxed, a negative weight cycle is present (not the case here).

**Code:**

```go
import "math"

func networkDelayTime(times [][]int, n int, k int) int {
    // Step 1: Initialize distances
    dist := make([]int, n+1)
    for i := 1; i <= n; i++ {
        dist[i] = math.MaxInt32
    }
    dist[k] = 0

    // Step 2: Relaxation for n-1 times
    for i := 0; i < n-1; i++ {
        for _, time := range times {
            u, v, w := time[0], time[1], time[2]
            if dist[u] != math.MaxInt32 && dist[u] + w < dist[v] {
                dist[v] = dist[u] + w
            }
        }
    }

    // Step 3: Calculate max path
    maxDist := 0
    for i := 1; i <= n; i++ {
        if dist[i] == math.MaxInt32 {
            return -1
        }
        if dist[i] > maxDist {
            maxDist = dist[i]
        }
    }

    return maxDist
}
```

**Complexity Analysis:**

- **Time Complexity:** \(O(V \times E)\), where \(V\) is the number of vertices and \(E\) is the number of edges.
- **Space Complexity:** \(O(V)\) for storing the distance array.

Both approaches solve the problem, with Dijkstra's being more efficient in terms of time complexity, given the non-negative weights, making it a more natural choice.

