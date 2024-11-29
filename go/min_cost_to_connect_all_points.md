# 1584. [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

## Approach 1: Prim's Algorithm 

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
import (
    "container/heap"
    "math"
)

func minCostConnectPoints(points [][]int) int {
    n := len(points)
    inMST := make([]bool, n)
    minHeap := &MinHeap{}
    heap.Init(minHeap)

    heap.Push(minHeap, &Edge{0, 0}) // (cost, point)

    totalCost := 0
    edgesUsed := 0

    for edgesUsed < n {
        top := heap.Pop(minHeap).(*Edge)
        cost := top.cost
        u := top.point

        if inMST[u] {
            continue // Skip if already included in the MST
        }

        inMST[u] = true
        totalCost += cost
        edgesUsed++

        for v := 0; v < n; v++ {
            if !inMST[v] {
                dist := abs(points[u][0]-points[v][0]) + abs(points[u][1]-points[v][1])
                heap.Push(minHeap, &Edge{dist, v})
            }
        }
    }

    return totalCost
}

type Edge struct {
    cost, point int
}

type MinHeap []*Edge

func (h MinHeap) Len() int            { return len(h) }
func (h MinHeap) Less(i, j int) bool  { return h[i].cost < h[j].cost }
func (h MinHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(*Edge)) }
func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[:n-1]
    return x
}

func abs(a int) int {
    if a < 0 {
        return -a
    }
    return a
}
```

## Approach 2: Kruskal's Algorithm with Union-Find

### Solution
go
```go
// Time Complexity: O(n^2 log n)
// Space Complexity: O(n^2)
import (
    "container/heap"
    "sort"
)

func minCostConnectPoints(points [][]int) int {
    n := len(points)
    edges := make([][]int, 0)

    // Calculate all possible edges and their costs
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            cost := abs(points[i][0]-points[j][0]) + abs(points[i][1]-points[j][1])
            edges = append(edges, []int{i, j, cost})
        }
    }

    // Sort edges based on cost
    sort.Slice(edges, func(i, j int) bool {
        return edges[i][2] < edges[j][2]
    })

    uf := NewUnionFind(n)
    totalCost := 0
    edgesUsed := 0

    for _, edge := range edges {
        if edgesUsed == n-1 {
            break
        }
        u, v, cost := edge[0], edge[1], edge[2]

        if uf.Union(u, v) {
            totalCost += cost
            edgesUsed++
        }
    }

    return totalCost
}

type UnionFind struct {
    parent, rank []int
}

func NewUnionFind(n int) *UnionFind {
    parent := make([]int, n)
    rank := make([]int, n)
    for i := 0; i < n; i++ {
        parent[i] = i
        rank[i] = 1
    }
    return &UnionFind{parent, rank}
}

func (uf *UnionFind) Find(x int) int {
    if uf.parent[x] != x {
        uf.parent[x] = uf.Find(uf.parent[x]) // Path compression
    }
    return uf.parent[x]
}

func (uf *UnionFind) Union(x, y int) bool {
    rootX := uf.Find(x)
    rootY := uf.Find(y)

    if rootX != rootY {
        if uf.rank[rootX] > uf.rank[rootY] {
            uf.parent[rootY] = rootX
        } else if uf.rank[rootX] < uf.rank[rootY] {
            uf.parent[rootX] = rootY
        } else {
            uf.parent[rootY] = rootX
            uf.rank[rootX]++
        }
        return true
    }

    return false
}

func abs(a int) int {
    if a < 0 {
        return -a
    }
    return a
}
```

