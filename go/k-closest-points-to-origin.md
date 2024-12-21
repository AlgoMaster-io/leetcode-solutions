# [Leetcode 973: K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approaches
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [Improved Sorting Approach](#approach-2-improved-sorting-approach)
3. [Optimized Priority Queue / Heap Approach](#approach-3-optimized-priority-queue--heap-approach)
4. [Quickselect Approach](#approach-4-quickselect-approach)

---

## Approach 1: Brute Force Approach

### Intuition:
The problem requires us to find the K closest points to the origin. A straightforward approach is to calculate the Euclidean distance for each point and store this information along with the point itself. Once we have the distances, we can then sort them and return the first K elements. 

### Steps:
1. Calculate the distance of each point from the origin using the formula: `distance = x^2 + y^2`.
2. Store each point along with its distance in a list.
3. Sort the list based on the distance.
4. Extract and return the first K points from the sorted list.

### Code:
```go
func kClosest(points [][]int, K int) [][]int {
    type Point struct {
        x, y, dist int
    }
    
    // Create a list of Points with calculated distances
    distances := make([]Point, len(points))
    for i, p := range points {
        distances[i] = Point{x: p[0], y: p[1], dist: p[0]*p[0] + p[1]*p[1]}
    }
    
    // Sort the points based on distance
    sort.Slice(distances, func(i, j int) bool {
        return distances[i].dist < distances[j].dist
    })
    
    // Collect the first K points
    result := make([][]int, K)
    for i := 0; i < K; i++ {
        result[i] = []int{distances[i].x, distances[i].y}
    }
    
    return result
}
```

### Complexity:
- **Time Complexity**: O(N log N), where N is the number of points, due to the sorting step.
- **Space Complexity**: O(N) for storing distances.

---

## Approach 2: Improved Sorting Approach

### Intuition:
The brute force solution can be slightly improved by using a more efficient sorting technique or choosing an in-place sort operation over the generated distances. However, as the sorting operation is required to determine the smallest K elements, we need to evaluate its complexity deeply.

### Steps:
- Use the same approach as the brute force except more efficiently handle the ordering or optimizing sort operation.

### Code:
The implementation might use similar Go standard library sorting functions, as shown in the brute force approach with slight optimizations.

### Complexity:
- **Time Complexity**: O(N log N), as sorting dominates.
- **Space Complexity**: O(N).

---

## Approach 3: Optimized Priority Queue / Heap Approach

### Intuition:
A better approach involves using a Max-Heap data structure to maintain the smallest K distances. By iterating through each point and maintaining a heap of size K, we ensure that we only keep track of the K closest points at any stage.

### Steps:
1. Use a max heap to keep track of the K closest points.
2. Push each point onto the heap, while maintaining only the top K elements (i.e., if the heap grows larger than K, remove the largest element).
3. Once iterated over all points, the heap will contain the K closest points.

### Code:
```go
import "container/heap"

type Point struct {
    x, y int
    dist int
}

type MaxHeap []Point

func (h MaxHeap) Len() int           { return len(h) }
func (h MaxHeap) Less(i, j int) bool { return h[i].dist > h[j].dist }
func (h MaxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MaxHeap) Push(x interface{}) {
    *h = append(*h, x.(Point))
}

func (h *MaxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    item := old[n-1]
    *h = old[0 : n-1]
    return item
}

func kClosest(points [][]int, K int) [][]int {
    maxHeap := &MaxHeap{}
    heap.Init(maxHeap)
    
    for _, p := range points {
        distance := p[0]*p[0] + p[1]*p[1]
        heap.Push(maxHeap, Point{x: p[0], y: p[1], dist: distance})
        if maxHeap.Len() > K {
            heap.Pop(maxHeap)
        }
    }
    
    result := make([][]int, K)
    for i := 0; i < K; i++ {
        point := heap.Pop(maxHeap).(Point)
        result[i] = []int{point.x, point.y}
    }
    
    return result
}
```

### Complexity:
- **Time Complexity**: O(N log K), where N is the number of points and K is the number closest points we want.
- **Space Complexity**: O(K) for the heap.

---

## Approach 4: Quickselect Approach

### Intuition:
Quickselect is a selection algorithm that can find the k-th smallest element in an unordered list. It’s similar to quicksort but instead of sorting the list, it only partially sorts to find the k-th smallest.

### Steps:
1. Randomly pick a pivot and partition the list based on the pivot's distance.
2. If the pivot is in position K-1, return the first K points.
3. Adjust the selection region based on the position of the pivot and repeat the partitioning process until the desired k-th order is achieved.

### Code:
```go
func kClosest(points [][]int, K int) [][]int {
    quickSelect(points, 0, len(points)-1, K)
    return points[:K]
}

func quickSelect(points [][]int, left, right, K int) {
    if left >= right {
        return
    }
    
    pivotIndex := partition(points, left, right)
    if pivotIndex == K {
        return
    } else if pivotIndex < K {
        quickSelect(points, pivotIndex+1, right, K)
    } else {
        quickSelect(points, left, pivotIndex-1, K)
    }
}

func partition(points [][]int, left, right int) int {
    pivot := points[right]
    pivotDist := pivot[0]*pivot[0] + pivot[1]*pivot[1]
    i := left

    for j := left; j < right; j++ {
        currentDist := points[j][0]*points[j][0] + points[j][1]*points[j][1]
        if currentDist < pivotDist {
            points[i], points[j] = points[j], points[i]
            i++
        }
    }
    
    points[i], points[right] = points[right], points[i]
    return i
}
```

### Complexity:
- **Time Complexity**: O(N) on average, O(N^2) in the worst case for the partitioning steps.
- **Space Complexity**: O(1), in-place partitioning.

---

Each of these approaches has its trade-offs in terms of simplicity, time complexity, and space complexity. Depending on the problem constraints and specifics of the use case, one might choose a particular solution. For large datasets with performance constraints, the quickselect approach typically offers an optimal balance.

