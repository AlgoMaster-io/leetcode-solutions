# [Leetcode 347: Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Approaches
- [Approach 1: Sorting with a Hash Map](#approach-1-sorting-with-a-hash-map)
- [Approach 2: Min-Heap](#approach-2-min-heap)
- [Approach 3: Bucket Sort](#approach-3-bucket-sort)

## Approach 1: Sorting with a Hash Map

### Intuition
The basic idea is to count the frequency of each element using a hash map and then sort the elements based on their frequencies. Once sorted, take the top k elements. This approach leverages sorting to arrange the elements by their frequency count.

### Algorithm
1. Create a hash map to count the frequency of each element.
2. Convert the hash map to a list of key-value pairs.
3. Sort the list based on frequency in descending order.
4. Extract the first k elements from the sorted list.

### Time Complexity
- **O(N log N)**: Due to the sorting operation, where N is the number of unique elements in the array.

### Space Complexity
- **O(N)**: For storing the frequency map and the sorted list of elements.

```go
func topKFrequent(nums []int, k int) []int {
    // Step 1: Create a frequency map
    freqMap := make(map[int]int)
    for _, num := range nums {
        freqMap[num]++
    }

    // Step 2: Convert map to a slice of pairs
    type pair struct {
        value int
        freq  int
    }
    freqPairs := make([]pair, 0, len(freqMap))
    for val, freq := range freqMap {
        freqPairs = append(freqPairs, pair{value: val, freq: freq})
    }

    // Step 3: Sort the pairs based on frequency
    sort.Slice(freqPairs, func(i, j int) bool {
        return freqPairs[i].freq > freqPairs[j].freq
    })

    // Step 4: Extract the top k elements
    result := make([]int, k)
    for i := 0; i < k; i++ {
        result[i] = freqPairs[i].value
    }
    return result
}
```

## Approach 2: Min-Heap

### Intuition
We can use a min-heap of size k to keep track of the k most frequent elements. By maintaining a min-heap, we ensure that at the end, we have the k most frequent elements in the heap because the minimum frequency element is always removed when the heap size exceeds k.

### Algorithm
1. Create a frequency map.
2. Use a min-heap to keep track of the top k elements. If the heap size exceeds k, remove the least frequent element.
3. The heap will contain the k most frequent elements.

### Time Complexity
- **O(N log k)**: Inserting into a heap takes O(log k) time and we do this for each of the N unique elements.

### Space Complexity
- **O(N + k)**: For the frequency map and the heap.

```go
func topKFrequent(nums []int, k int) []int {
    // Step 1: Create a frequency map
    freqMap := make(map[int]int)
    for _, num := range nums {
        freqMap[num]++
    }

    // Step 2: Create a min-heap with custom comparator
    h := &IntHeap{}
    heap.Init(h)

    // Step 3: Iterate over the frequency map to populate the heap
    for val, freq := range freqMap {
        heap.Push(h, [2]int{freq, val})
        if h.Len() > k {
            heap.Pop(h)
        }
    }

    // Step 4: Extract elements from the heap
    result := make([]int, k)
    for i := 0; h.Len() > 0; i++ {
        result[i] = heap.Pop(h).([2]int)[1]
    }
    return result
}

// IntHeap is a custom struct implementing a min-heap based on the frequency
type IntHeap [][2]int

func (h IntHeap) Len() int            { return len(h) }
func (h IntHeap) Less(i, j int) bool  { return h[i][0] < h[j][0] }
func (h IntHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *IntHeap) Push(x interface{}) { *h = append(*h, x.([2]int)) }
func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

## Approach 3: Bucket Sort

### Intuition
This approach uses bucket sort. The idea is to use the frequency as an index into a bucket array, where each bucket corresponds to the elements that have the same frequency. By traversing the bucket array from the end to the start, we can easily gather the top k frequent elements.

### Algorithm
1. Create a frequency map.
2. Create a slice of slices (buckets) where index corresponds to frequency.
3. Append elements to their corresponding frequency bucket.
4. Traverse buckets starting from the end and gather top k elements.

### Time Complexity
- **O(N)**: Collecting frequencies and placing elements into buckets is linear with respect to the number of elements.

### Space Complexity
- **O(N)**: For storing the frequency map and the bucket array.

```go
func topKFrequent(nums []int, k int) []int {
    // Step 1: Create a frequency map
    freqMap := make(map[int]int)
    for _, num := range nums {
        freqMap[num]++
    }

    // Step 2: Create buckets array where index represents frequency
    buckets := make([][]int, len(nums)+1)
    for val, freq := range freqMap {
        buckets[freq] = append(buckets[freq], val)
    }

    // Step 3: Gather top k elements from the buckets
    result := make([]int, 0, k)
    for i := len(buckets) - 1; i >= 0 && len(result) < k; i-- {
        result = append(result, buckets[i]...)
    }
    
    // Step 4: Make sure only the top k elements are returned
    return result[:k]
}
```

This completion provides a detailed solution to the problem "Top K Frequent Elements" using various approaches from basic to optimal, described with intuitions, code, and complexities.

