# [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approaches
- [Approach 1: Naive approach using a sorted array](#approach-1-naive-approach-using-a-sorted-array)
- [Approach 2: Using Min-Heap](#approach-2-using-min-heap)

### Approach 1: Naive Approach using a Sorted Array

#### Intuition
- The naive approach involves maintaining a sorted array representing the stream of numbers. For every `add` operation, insert the number and sort the array.
- The K-th largest element will then simply be at the position `len(array) - k`.

#### Steps
1. Initialize an empty array to hold the elements.
2. Insert each element from the initial array `nums` into it and sort the array.
3. For each call to `add(val)`, append `val` to the array, sort it, and return the element at position `len(array) - k`.

#### Code
```go
type KthLargest struct {
    k    int
    nums []int
}

func Constructor(k int, nums []int) KthLargest {
    kl := KthLargest{k: k, nums: nums}
    sort.Ints(kl.nums) // sort the initial array
    return kl
}

func (kl *KthLargest) Add(val int) int {
    kl.nums = append(kl.nums, val) // Add new value
    sort.Ints(kl.nums) // Sort the array
    return kl.nums[len(kl.nums)-kl.k] // Return the k-th largest
}
```

#### Complexity Analysis
- **Time Complexity**: `O(n log n)` for sorting with each addition, where `n` is the number of elements currently in the stream.
- **Space Complexity**: `O(n)` to hold the array of numbers.

### Approach 2: Using Min-Heap

#### Intuition
- This optimal approach uses a Min-Heap to keep track of the `k` largest elements. This allows insertion and extraction of elements to be performed efficiently.
- The Min-Heap will have a size of `k`, where the root is the K-th largest element in the stream.

#### Steps
1. Initialize a Min-Heap with the first `k` numbers of the input array.
2. For each additional element (from `nums` or a new `add(val)`), check if it is greater than the minimum (root) of the heap.
3. If yes, remove the smallest and insert the new number to maintain only `k` largest elements.

#### Code
```go
type KthLargest struct {
    k    int
    heap []int
}

func Constructor(k int, nums []int) KthLargest {
    kl := KthLargest{k: k, heap: []int{}}
    for _, num := range nums {
        kl.Add(num)
    }
    return kl
}

func (kl *KthLargest) Add(val int) int {
    if len(kl.heap) < kl.k {
        heap.Push(&kl, val)
    } else if val > kl.heap[0] {
        heap.Pop(&kl)
        heap.Push(&kl, val)
    }
    return kl.heap[0]
}

// Implementation of heap.Interface
func (kl KthLargest) Len() int            { return len(kl.heap) }
func (kl KthLargest) Less(i, j int) bool  { return kl.heap[i] < kl.heap[j] }
func (kl KthLargest) Swap(i, j int)       { kl.heap[i], kl.heap[j] = kl.heap[j], kl.heap[i] }
func (kl *KthLargest) Push(x interface{}) { kl.heap = append(kl.heap, x.(int)) }
func (kl *KthLargest) Pop() interface{} {
    old := kl.heap
    n := len(old)
    x := old[n-1]
    kl.heap = old[0 : n-1]
    return x
}
```

#### Complexity Analysis
- **Time Complexity**: `O(log k)` for each insertion, as the size of the heap is always `k`.
- **Space Complexity**: `O(k)` since at most `k` elements are stored in the heap.

