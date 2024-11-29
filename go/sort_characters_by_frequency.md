# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approach 1: Using HashMap and Sorting

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import "container/heap"
import "strings"

type CharFrequency struct {
    char  rune
    count int
}

type MaxHeap []CharFrequency

func (h MaxHeap) Len() int           { return len(h) }
func (h MaxHeap) Less(i, j int) bool { return h[i].count > h[j].count }
func (h MaxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MaxHeap) Push(x interface{}) {
    *h = append(*h, x.(CharFrequency))
}

func (h *MaxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func frequencySort(s string) string {
    // Count the frequency of each character
    frequencyMap := make(map[rune]int)
    for _, c := range s {
        frequencyMap[c]++
    }

    // Sort characters by frequency in descending order using max heap
    maxHeap := &MaxHeap{}
    heap.Init(maxHeap)
    for char, count := range frequencyMap {
        heap.Push(maxHeap, CharFrequency{char, count})
    }

    // Build the result string
    var result strings.Builder
    for maxHeap.Len() > 0 {
        entry := heap.Pop(maxHeap).(CharFrequency)
        for i := 0; i < entry.count; i++ {
            result.WriteRune(entry.char)
        }
    }

    return result.String()
}
```

## Approach 2: Bucket Sort (Optimized for Large Inputs)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
import "strings"

func frequencySort(s string) string {
    // Count the frequency of each character
    frequency := make([]int, 128) // Assuming ASCII characters
    for _, c := range s {
        frequency[c]++
    }

    // Create buckets where index represents frequency
    buckets := make([][]rune, len(s)+1)
    for i, count := range frequency {
        if count > 0 {
            if buckets[count] == nil {
                buckets[count] = make([]rune, 0)
            }
            buckets[count] = append(buckets[count], rune(i))
        }
    }

    // Build the result string from the buckets
    var result strings.Builder
    for i := len(buckets) - 1; i > 0; i-- {
        if buckets[i] != nil {
            for _, c := range buckets[i] {
                for j := 0; j < i; j++ {
                    result.WriteRune(c)
                }
            }
        }
    }

    return result.String()
}
```

