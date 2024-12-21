# [Leetcode 857: Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approaches
1. [Brute Force with Permutations](#approach-1-brute-force-with-permutations)
2. [Greedy with Sorting and Priority Queue](#approach-2-greedy-with-sorting-and-priority-queue)

---

## Approach 1: Brute Force with Permutations

### Intuition:
The basic idea of this approach is to generate all possible combinations of k workers from the provided list and calculate the cost for each permutation. The goal is to select the k workers whose combined cost is the minimum amongst all possible choices. This solution, while accurate, is computationally expensive and not feasible for larger inputs due to the large number of permutations.

### Time Complexity:
- The time complexity is \(O(n^k \cdot k)\), where n is the number of workers and k is the required number of workers we need to hire. This is due to the exploration of all permutations of length k.

### Space Complexity:
- The space complexity is \(O(n)\) for storing intermediate lists of workers.

### Implementation:

```go
import "math"

// A helper function to calculate wage by using the selected k workers
func calculateWage(quality []int, wage []int, selected []int) float64 {
    totalQuality := 0
    maxRatio := 0.0

    // Summing quality and finding the maximum ratio
    for _, i := range selected {
        totalQuality += quality[i]
        ratio := float64(wage[i]) / float64(quality[i])
        if ratio > maxRatio {
            maxRatio = ratio
        }
    }
    return float64(totalQuality) * maxRatio
}

// Brute Force function to find minimum cost to hire k workers
func mincostToHireWorkersBruteForce(quality []int, wage []int, K int) float64 {
    n := len(quality)
    minCost := math.MaxFloat64

    var permute func(current []int, start int)
    permute = func(current []int, start int) {
        if len(current) == K {
            cost := calculateWage(quality, wage, current)
            if cost < minCost {
                minCost = cost
            }
            return
        }

        for i := start; i < n; i++ {
            permute(append(current, i), i+1)
        }
    }

    permute([]int{}, 0)
    return minCost
}
```

---

## Approach 2: Greedy with Sorting and Priority Queue

### Intuition:
This approach involves sorting the workers based on their ratio of wage to quality in ascending order. By selecting workers starting from the lowest ratio, we ensure that we can satisfy their minimum wage requirement while possibly minimizing the overall cost. We use a max-heap to efficiently maintain and remove the highest quality workers from our current selection whenever needed to reach the minimum cost.

### Time Complexity:
- The time complexity is \(O(n \log n)\) due to sorting and using a priority queue.

### Space Complexity:
- The space complexity is \(O(k)\) for the priority queue.

### Implementation:

```go
import (
    "container/heap"
    "math"
    "sort"
)

// Worker struct to keep track of quality, wage, and ratio
type Worker struct {
    quality int
    wage    int
    ratio   float64
}

// Max heap to store quality
type MaxHeap []int

func (h MaxHeap) Len() int            { return len(h) }
func (h MaxHeap) Less(i, j int) bool  { return h[i] > h[j] }
func (h MaxHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MaxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

// Optimal function to find minimum cost to hire k workers using greedy method.
func mincostToHireWorkersGreedy(quality []int, wage []int, K int) float64 {
    n := len(quality)
    workers := make([]Worker, n)

    // Build the worker array with their respective ratio
    for i := 0; i < n; i++ {
        workers[i] = Worker{quality: quality[i], wage: wage[i], ratio: float64(wage[i]) / float64(quality[i])}
    }

    // Sort workers based on the ratio
    sort.Slice(workers, func(i, j int) bool {
        return workers[i].ratio < workers[j].ratio
    })

    var qualitySum int
    minCost := math.MaxFloat64
    maxQualityHeap := &MaxHeap{}

    // Iterate over sorted ratios and calculate the minimum cost to hire k workers
    for _, worker := range workers {
        heap.Push(maxQualityHeap, worker.quality)
        qualitySum += worker.quality

        if maxQualityHeap.Len() > K {
            qualitySum -= heap.Pop(maxQualityHeap).(int)
        }

        if maxQualityHeap.Len() == K {
            cost := float64(qualitySum) * worker.ratio
            if cost < minCost {
                minCost = cost
            }
        }
    }

    return minCost
}
```

Using a greedy approach with a priority queue allows us to handle larger inputs efficiently by maintaining the smallest total cost dynamically as we explore each subset of potential workers.

