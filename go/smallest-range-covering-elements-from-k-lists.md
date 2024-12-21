### [LeetCode 632: Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/)

---

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Min-Heap and Two-Pointer Technique](#approach-2-min-heap-and-two-pointer-technique)

---

## Approach 1: Brute Force

### Intuition

The brute force approach involves considering all possible ranges formed by picking one element from each list and determining the smallest range.

### Steps

1. Generate all possible combinations of elements, taking one from each list. This can be achieved using nested iterations.
2. For each combination, calculate the range (difference between the maximum and minimum elements).
3. Track the smallest range found.

### Code

```go
func smallestRange(nums [][]int) []int {
    // Initialize variables to track the smallest range found
    minRange := []int{-1e5, 1e5}
    rangeLen := minRange[1] - minRange[0]
    
    // Calculate the lengths of each list
    n := len(nums)
    indices := make([]int, n)

    // Function to determine if all indices are valid
    isValid := func() bool {
        for i := range indices {
            if indices[i] >= len(nums[i]) {
                return false
            }
        }
        return true
    }
    
    // Exhaustively try each combination of indices
    for isValid() {
        currentMin, currentMax := 1e5, -1e5
        // Find min and max for the current combination
        for i := 0; i < n; i++ {
            if nums[i][indices[i]] < currentMin {
                currentMin = nums[i][indices[i]]
            }
            if nums[i][indices[i]] > currentMax {
                currentMax = nums[i][indices[i]]
            }
        }
        
        // Check if the range of current combination is smaller
        currentRangeLen := currentMax - currentMin
        if currentRangeLen < rangeLen {
            minRange[0], minRange[1] = currentMin, currentMax
            rangeLen = currentRangeLen
        }
        
        // Try the next combination
        for i := n - 1; i >= 0; i-- {
            if indices[i] + 1 < len(nums[i]) {
                indices[i]++
                break
            } else {
                indices[i] = 0
            }
        }
    }
    
    return minRange
}
```

### Complexity

- **Time Complexity**: \(O(N^K)\), where \(N\) is the average length of each list and \(K\) is the number of lists.
- **Space Complexity**: \(O(K)\) due to the storage of indices.

---

## Approach 2: Min-Heap and Two-Pointer Technique

### Intuition

This approach leverages a min-heap to efficiently manage the minimums while using two pointers to manage and adjust the range. By always pushing the next element of the list from which the smallest element was taken, we ensure that all lists are covered while attempting to keep the range small.

### Steps

1. Use a min-heap to track the smallest element from each list.
2. Simultaneously maintain a high-pointer to track the current maximum element seen (initialized to the max of the elements first seen).
3. Extract the minimum from the heap, attempt to update the range if it's smaller.
4. Replace the extracted minimum with the next element from its respective list.
5. Repeat until you exhaust one of the lists.

### Code

```go
import (
    "container/heap"
    "math"
)

type Element struct {
    value int
    listIndex int
    elementIndex int
}

type MinHeap []Element

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].value < h[j].value }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) {
    *h = append(*h, x.(Element))
}
func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func smallestRange(nums [][]int) []int {
    minHeap := &MinHeap{}
    heap.Init(minHeap)
    maxNum := math.MinInt32

    // Add the first element of each list to the min-heap
    for i := 0; i < len(nums); i++ {
        heap.Push(minHeap, Element{value: nums[i][0], listIndex: i, elementIndex: 0})
        if nums[i][0] > maxNum {
            maxNum = nums[i][0]
        }
    }

    minRange := []int{-1e5, 1e5}
    rangeLen := minRange[1] - minRange[0]

    for {
        minElement := heap.Pop(minHeap).(Element)
        currentMin, currentMax := minElement.value, maxNum

        // Update the range if it's smaller
        if currentMax - currentMin < rangeLen {
            minRange[0], minRange[1] = currentMin, currentMax
            rangeLen = currentMax - currentMin
        }

        // Get the next element in the current list
        if minElement.elementIndex+1 < len(nums[minElement.listIndex]) {
            nextElement := nums[minElement.listIndex][minElement.elementIndex+1]
            heap.Push(minHeap, Element{value: nextElement, listIndex: minElement.listIndex, elementIndex: minElement.elementIndex+1})

            if nextElement > maxNum {
                maxNum = nextElement
            }
        } else {
            break // One of the lists is exhausted
        }
    }

    return minRange
}
```

### Complexity

- **Time Complexity**: \(O(N \log K)\), where \(N\) is the total number of elements across all lists and \(K\) is the number of lists.
- **Space Complexity**: \(O(K)\) due to the storage of the min-heap.

