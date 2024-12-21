# [Leetcode 373: Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach using Min-Heap](#optimized-approach-using-min-heap)

### Brute Force Approach

**Intuition:**

The brute force solution involves generating all possible pairs from the two arrays, calculating their sums, and then sorting these sums to find the k smallest ones. This approach is straightforward but can be inefficient, especially for larger arrays, as it involves examining every possible pair.

**Steps:**
1. Initialize an empty list to store pairs along with their sums.
2. Iterate over all elements in both arrays, generating pairs.
3. Calculate the sum for each pair and store it in a list.
4. Sort this list by the sums.
5. Return the first k pairs from the sorted list.

**Time Complexity:** O(n * m + k log k)  
**Space Complexity:** O(n * m)

```go
func kSmallestPairs(nums1 []int, nums2 []int, k int) [][]int {
    pairs := [][]int{}

    // Generate all possible pairs with their sums
    for _, num1 := range nums1 {
        for _, num2 := range nums2 {
            pairs = append(pairs, []int{num1, num2})
        }
    }

    // Sort pairs based on the sum of elements
    sort.Slice(pairs, func(i, j int) bool {
        return pairs[i][0]+pairs[i][1] < pairs[j][0]+pairs[j][1]
    })

    // Get the first k pairs
    if k > len(pairs) {
        k = len(pairs)
    }
    return pairs[:k]
}
```

### Optimized Approach using Min-Heap

**Intuition:**

The optimized approach leverages a min-heap (priority queue) to efficiently find the k smallest sums by gradually exploring potential pairs. By using a heap, we can continuously extract the smallest available sum and add potential next candidates.

**Steps:**
1. Use a priority queue (min-heap) to store pairs alongside their sums.
2. Initialize the heap with pairs formed by the smallest element in nums1 and every element in nums2. 
3. Extract the smallest sum and add the next possible pair from nums1 (incrementing the index in nums1).
4. Continue extracting k times or until no more elements can form a valid pair.

**Time Complexity:** O(k log k)  
**Space Complexity:** O(k)

```go
import (
    "container/heap"
)

type Pair struct {
    sum  int
    i, j int
}

type MinHeap []Pair

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].sum < h[j].sum }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
    *h = append(*h, x.(Pair))
}

func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func kSmallestPairs(nums1 []int, nums2 []int, k int) [][]int {
    minHeap := &MinHeap{}
    heap.Init(minHeap)

    // Initialize the min-heap with first element of nums1 and all elements of nums2
    for j := 0; j < len(nums2) && j < k; j++ {
        heap.Push(minHeap, Pair{nums1[0] + nums2[j], 0, j})
    }

    result := [][]int{}

    for k > 0 && minHeap.Len() > 0 {
        k--
        p := heap.Pop(minHeap).(Pair)
        result = append(result, []int{nums1[p.i], nums2[p.j]})

        // If possible, push the next pair in nums1 for the current j
        if p.i+1 < len(nums1) {
            heap.Push(minHeap, Pair{nums1[p.i+1] + nums2[p.j], p.i + 1, p.j})
        }
    }
    return result
}
```

This code efficiently finds the k pairs with the smallest sums. By understanding the problem using both the brute force and optimized approaches, we can grasp the value of algorithms in reducing computational effort.

