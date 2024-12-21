# [1584. Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

## Approaches
1. [Kruskal's Algorithm using Union-Find](#kruskals-algorithm-using-union-find)
2. [Prim's Algorithm using Min-Heap](#prims-algorithm-using-min-heap)

### Kruskal's Algorithm using Union-Find

Kruskal's algorithm is a greedy algorithm that finds a minimum spanning tree for a connected weighted graph. It works by sorting all the edges in increasing order and adding them one by one, as long as they don't form a cycle, using the Union-Find data structure for cycle detection.

#### Intuition

1. **Calculate all possible edges**: For `n` points, we calculate the Manhattan distance between every pair `(i, j)`. This distance acts as the weight of the edge connecting points `i` and `j`.
2. **Sort edges by weight**: Sort these edges based on their weights (distances).
3. **Union-Find data structure**: Utilize a Union-Find structure to keep track of connected components.
4. **Select edges**: Traverse through the sorted edges, adding an edge to the MST if it doesn’t form a cycle (i.e., the two points are already not connected).

#### Time and Space Complexity

- **Time Complexity**: O(E log E), where E is the total number of edges. Sorting the edges takes O(E log E), and iterating over them for Union-Find operations is almost linear in E, thanks to path compression.
- **Space Complexity**: O(V), where V is the number of nodes (or points), due to the Union-Find data structure.

```go
import (
    "sort"
)

type Edge struct {
    weight int
    u, v   int
}

type UnionFind struct {
    parent []int
    rank   []int
}

func (uf *UnionFind) find(x int) int {
    if uf.parent[x] != x {
        uf.parent[x] = uf.find(uf.parent[x]) // Path compression
    }
    return uf.parent[x]
}

func (uf *UnionFind) union(x, y int) bool {
    rootX := uf.find(x)
    rootY := uf.find(y)
    if rootX == rootY {
        return false
    }
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

// Calculates the Manhattan distance between two points
func calculateDistance(point1, point2 []int) int {
    return abs(point1[0]-point2[0]) + abs(point1[1]-point2[1])
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}

func minCostConnectPoints(points [][]int) int {
    n := len(points)
    edges := make([]Edge, 0)
    
    // Create all possible edges with their respective weights
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            distance := calculateDistance(points[i], points[j])
            edges = append(edges, Edge{weight: distance, u: i, v: j})
        }
    }
    
    // Sort edges based on their weight (ascending order)
    sort.Slice(edges, func(i, j int) bool {
        return edges[i].weight < edges[j].weight
    })
    
    uf := UnionFind{parent: make([]int, n), rank: make([]int, n)}
    for i := 0; i < n; i++ {
        uf.parent[i] = i
    }

    minCost := 0
    edgesUsed := 0
    
    for _, edge := range edges {
        if uf.union(edge.u, edge.v) { // If u and v are in different components
            minCost += edge.weight
            edgesUsed++
            if edgesUsed == n-1 { // MST found when we have n-1 edges
                break
            }
        }
    }
    
    return minCost
}
```

### Prim's Algorithm using Min-Heap

Prim's algorithm is another greedy algorithm that builds the minimum spanning tree by growing the MST from an arbitrary starting point and always adding the minimum weight edge from the tree to a vertex that isn't in the tree yet.

#### Intuition

1. **Initialize**: Start from an arbitrary point and add it to the MST.
2. **Min-Heap**: Use a min-heap to efficiently fetch the cheapest edge that expands the MST. 
3. **Iterate until fully connected**: Extract the smallest edge and use it to grow the MST, repeating this process until all points are included.

#### Time and Space Complexity

- **Time Complexity**: O(V^2 log V), where V is the number of points. In a dense graph (complete graph), Prim's algorithm with a priority queue for edges is roughly O(V^2).
- **Space Complexity**: O(V^2) due to the storage of all possible edges (though this can be optimized with adjacency lists).

```go
import (
    "container/heap"
)

type Item struct {
    cost  int
    point int
}

// A PriorityQueue implements heap.Interface and holds Items.
type PriorityQueue []Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool { return pq[i].cost < pq[j].cost }

func (pq PriorityQueue) Swap(i, j int) { pq[i], pq[j] = pq[j], pq[i] }

func (pq *PriorityQueue) Push(x interface{}) { *pq = append(*pq, x.(Item)) }

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    *pq = old[0 : n-1]
    return item
}

func primMinCostConnectPoints(points [][]int) int {
    n := len(points)
    if n == 0 {
        return 0
    }
    
    minCost := 0
    visited := make([]bool, n)
    minHeap := &PriorityQueue{}
    heap.Init(minHeap)
    
    // Start from the first point
    heap.Push(minHeap, Item{cost: 0, point: 0})
    
    for minHeap.Len() > 0 {
        item := heap.Pop(minHeap).(Item)
        if visited[item.point] {
            continue
        }
        
        // Mark this point as visited
        visited[item.point] = true
        minCost += item.cost
        
        // Visit neighbors not yet in the MST
        for j := 0; j < n; j++ {
            if !visited[j] {
                dist := calculateDistance(points[item.point], points[j])
                heap.Push(minHeap, Item{cost: dist, point: j})
            }
        }
    }
    
    return minCost
}
```

These two graph-based algorithms tackle the problem differently but effectively achieve the task, each with its trade-offs in terms of simplicity, flexibility, and system constraints. Kruskal's is excellent when edges are sparse and can be sorted quickly, while Prim's algorithm shines in dense graphs thanks to the use of efficient heaps.

