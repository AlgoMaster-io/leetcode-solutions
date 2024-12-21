# [Leetcode 480: Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approaches
- [Naive Approach: Sorting for Each Window](#naive-approach-sorting-for-each-window)
- [Optimized Approach: Two Heaps (Min-Heap and Max-Heap)](#optimized-approach-two-heaps-min-heap-and-max-heap)

---

### Naive Approach: Sorting for Each Window

#### Intuition:
The simplest approach to solve the sliding window median problem is to maintain a sliding window of numbers, sort them, and then find the median for each window. This is intuitive and straightforward but not efficient for larger windows due to the overhead of sorting.

#### Steps:
1. Iterate through the array with a sliding window of size `k`.
2. For each start position of the window, extract the subarray of size `k`.
3. Sort the subarray and compute the median.
4. Continue this process for all possible positions of the sliding window.

```go
func medianSlidingWindow(nums []int, k int) []float64 {
    var result []float64

    for i := 0; i <= len(nums) - k; i++ {
        window := make([]int, k)
        copy(window, nums[i:i+k])
        
        // Sort the window to find the median directly
        sort.Ints(window)
        
        if k%2 == 0 {
            // If even, median is the average of the two middle numbers
            median := float64(window[k/2-1]+window[k/2]) / 2.0
            result = append(result, median)
        } else {
            // If odd, median is the middle number
            median := float64(window[k/2])
            result = append(result, median)
        }
    }
    
    return result
}
```
#### Time Complexity:
- O((n-k+1) * k * log(k)): For each window position, sorting takes O(k*log(k)) and we do this for (n-k+1) positions.

#### Space Complexity:
- O(k): To store the current window.

---

### Optimized Approach: Two Heaps (Min-Heap and Max-Heap)

#### Intuition:
To avoid the inefficiencies of sorting each window, we can use two heaps to efficiently get the median:
- A max heap to maintain the lower half of the numbers.
- A min heap to maintain the upper half of the numbers.
- The median can be easily calculated by peeking the tops of the heaps.

#### Steps:
1. Use a max heap for the lower half and a min heap for the upper half.
2. As you slide the window, maintain the balance of the heaps.
3. If the number of elements is odd, the max heap will have one more element than the min heap.
4. The median is either the maximum of the max heap (when odd) or the average of the max of the max heap and the min of the min heap (when even).
5. Carefully handle the addition and removal of elements from each heap while maintaining the balance.

```go
import (
    "container/heap"
)

// MinHeap implementation
type MinHeap []int

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MinHeap) Pop() interface{} {
    n := len(*h)
    x := (*h)[n-1]
    *h = (*h)[:n-1]
    return x
}

// MaxHeap implementation
type MaxHeap []int

func (h MaxHeap) Len() int           { return len(h) }
func (h MaxHeap) Less(i, j int) bool { return h[i] > h[j] }
func (h MaxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MaxHeap) Pop() interface{} {
    n := len(*h)
    x := (*h)[n-1]
    *h = (*h)[:n-1]
    return x
}

func medianSlidingWindow(nums []int, k int) []float64 {
    minHeap := &MinHeap{}
    maxHeap := &MaxHeap{}
    heap.Init(minHeap)
    heap.Init(maxHeap)
    
    var result []float64
    
    removeMap := make(map[int]int) // To track invalid elements

    // Function to rebalance heaps
    rebalance := func() {
        for minHeap.Len() > 0 && removeMap[(*minHeap)[0]] > 0 {
            removeMap[(*minHeap)[0]]--
            heap.Pop(minHeap)
        }
        for maxHeap.Len() > 0 && removeMap[(*maxHeap)[0]] > 0 {
            removeMap[(*maxHeap)[0]]--
            heap.Pop(maxHeap)
        }
        
        // Balance heaps if needed
        for maxHeap.Len() > minHeap.Len()+1 {
            heap.Push(minHeap, heap.Pop(maxHeap))
        }
        
        for minHeap.Len() > maxHeap.Len() {
            heap.Push(maxHeap, heap.Pop(minHeap))
        }
    }

    for i := 0; i < len(nums); i++ {
        if maxHeap.Len() == 0 || nums[i] <= (*maxHeap)[0] {
            heap.Push(maxHeap, nums[i])
        } else {
            heap.Push(minHeap, nums[i])
        }
        
        // Remove the element that is sliding out of the window
        if i >= k {
            removeMap[nums[i-k]]++
            if nums[i-k] <= (*maxHeap)[0] {
                removeMap[nums[i-k]]++
            }
        }
        
        // Rebalance heaps
        rebalance()
        
        // Calculate median
        if i >= k-1 {
            if k%2 == 0 {
                median := float64((*maxHeap)[0]+(*minHeap)[0]) / 2.0
                result = append(result, median)
            } else {
                median := float64((*maxHeap)[0])
                result = append(result, median)
            }
        }
    }

    return result
}
```
#### Time Complexity:
- O(n * log(k)): Inserting, removing, and rebalancing each take O(log(k)) per element.

#### Space Complexity:
- O(k): To store the k elements in the heaps.

The two heaps solution optimizes the process by reducing the need to sort the array repeatedly, making it suitable for large data sets. This is achieved by managing the elements effectively using two heaps.

