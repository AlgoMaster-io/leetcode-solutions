# [692. Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

## Approach 1: HashMap and Min-Heap

### Solution
```go
// Time Complexity: O(n log k)
// Space Complexity: O(n)
import (
    "container/heap"
    "sort"
)

type WordFrequency struct {
    Word     string
    Frequency int
}

type MinHeap []WordFrequency

func (h MinHeap) Len() int {
    return len(h)
}

func (h MinHeap) Less(i, j int) bool {
    if h[i].Frequency == h[j].Frequency {
        return h[j].Word < h[i].Word // Sort lexicographically for equal frequency
    }
    return h[i].Frequency < h[j].Frequency
}

func (h MinHeap) Swap(i, j int) {
    h[i], h[j] = h[j], h[i]
}

func (h *MinHeap) Push(x interface{}) {
    *h = append(*h, x.(WordFrequency))
}

func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func topKFrequent(words []string, k int) []string {
    // Count the frequency of each word
    frequencyMap := make(map[string]int)
    for _, word := range words {
        frequencyMap[word]++
    }

    // Use a min-heap to keep the top k frequent words
    minHeap := &MinHeap{}
    heap.Init(minHeap)
    
    for word, freq := range frequencyMap {
        heap.Push(minHeap, WordFrequency{Word: word, Frequency: freq})
        if minHeap.Len() > k {
            heap.Pop(minHeap)
        }
    }

    // Build the result list from the heap
    result := make([]string, k)
    for i := k - 1; i >= 0; i-- {
        result[i] = heap.Pop(minHeap).(WordFrequency).Word
    }
    return result
}
```

## Approach 2: HashMap and Sorting

### Solution
```go
// Time Complexity: O(n + k log k)
// Space Complexity: O(n)
import (
    "sort"
)

func topKFrequent(words []string, k int) []string {
    // Count the frequency of each word
    frequencyMap := make(map[string]int)
    for _, word := range words {
        frequencyMap[word]++
    }

    // Sort the words by frequency and lexicographical order
    candidates := make([]string, 0, len(frequencyMap))
    for word := range frequencyMap {
        candidates = append(candidates, word)
    }
    
    sort.Slice(candidates, func(i, j int) bool {
        if frequencyMap[candidates[i]] == frequencyMap[candidates[j]] {
            return candidates[i] < candidates[j] // Lexicographical order for equal frequency
        }
        return frequencyMap[candidates[i]] > frequencyMap[candidates[j]] // Higher frequency first
    })

    // Return the top k words
    return candidates[:k]
}
```

