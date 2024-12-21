# [Leetcode 378: Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)

## Approaches:
- [Approach 1: Brute Force with Sorting](#approach-1)
- [Approach 2: Min-Heap](#approach-2)
- [Approach 3: Binary Search on Value Range](#approach-3)

### Approach 1: Brute Force with Sorting

#### Intuition:
The simplest way to solve this problem is to convert the 2D matrix into a 1D array, sort it, and then retrieve the k-th smallest element. Given the properties of the matrix, this will guarantee the correct answer.

#### Steps:
1. Flatten the matrix into a single list.
2. Sort the list.
3. Return the element at the (k-1)th index of the sorted list.

#### Time Complexity:
- **O(N^2 log N)**: Where N is the number of rows (or columns) of the matrix. Flattening takes O(N^2), sorting takes O(N^2 log N).

#### Space Complexity:
- **O(N^2)**: For storing the flattened matrix.

```go
func kthSmallest(matrix [][]int, k int) int {
    nums := []int{}
    // Flatten the matrix
    for _, row := range matrix {
        nums = append(nums, row...)
    }
    // Sort the flattened array
    sort.Ints(nums)
    // Return the k-th smallest element
    return nums[k-1]
}
```

### Approach 2: Min-Heap

#### Intuition:
Given each row (and column) is sorted, the smallest element in the matrix will be at the top left corner. We can use a Min-Heap to efficiently extract the smallest elements repeatedly.

#### Steps:
1. Push the first element of each row onto the Min-Heap.
2. Perform k-1 pop and push operations: 
   - Extract the minimum element (current smallest)
   - Push the next element from the same row as the extracted element.
3. The k-th pop will give the k-th smallest element.

#### Time Complexity:
- **O(k log N)**: At most, we perform k extract-min operations and there are N elements in the heap.

#### Space Complexity:
- **O(N)**: Space required to store N elements in the heap.

```go
type Item struct {
    val, row, col int
}

func kthSmallest(matrix [][]int, k int) int {
    minHeap := &MinHeap{}
    heap.Init(minHeap)
    
    // Initialize the heap with the first element of each row
    for r := 0; r < len(matrix); r++ {
        heap.Push(minHeap, &Item{val: matrix[r][0], row: r, col: 0})
    }
    
    // Extract Minimum k times
    for i := 0; i < k-1; i++ {
        item := heap.Pop(minHeap).(*Item)
        nextCol := item.col + 1
        if nextCol < len(matrix[item.row]) {
            heap.Push(minHeap, &Item{val: matrix[item.row][nextCol], row: item.row, col: nextCol})
        }
    }
    
    return heap.Pop(minHeap).(*Item).val
}

type MinHeap []*Item

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].val < h[j].val }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
    *h = append(*h, x.(*Item))
}

func (h *MinHeap) Pop() interface{} {
    old := *h
    n := len(old)
    item := old[n-1]
    *h = old[0 : n-1]
    return item
}
```

### Approach 3: Binary Search on Value Range

#### Intuition:
Instead of sorting or maintaining a heap, leverage the properties of the matrix:
- We know the matrix is sorted both row-wise and column-wise.
- Use binary search to narrow down the range between the smallest and largest possible values in the matrix.
  
By counting how many numbers are less than or equal to a mid-value and adjusting the search range based on k, this approach can efficiently find the k-th smallest.

#### Steps:
1. Initialize `left` to the smallest element and `right` to the largest element in the matrix.
2. While `left` < `right`:
   - Calculate `mid`.
   - Count elements less than or equal to `mid`.
   - Adjust `left` or `right` based on the k comparison.
3. `left` will be at the k-th smallest value.

#### Time Complexity:
- **O(N log(max-min))**: Binary search over `max-min` times N element comparisons.

#### Space Complexity:
- **O(1)**: Additional space used is constant.

```go
func kthSmallest(matrix [][]int, k int) int {
    n := len(matrix)
    left, right := matrix[0][0], matrix[n-1][n-1]
    
    for left < right {
        mid := left + (right-left)/2
        count := countLessEqual(matrix, mid)
        
        if count < k {
            left = mid + 1
        } else {
            right = mid
        }
    }
    
    return left
}

func countLessEqual(matrix [][]int, x int) int {
    count, n := 0, len(matrix)
    row, col := n-1, 0
    for row >= 0 && col < n {
        if matrix[row][col] <= x {
            // Include all elements of the current column which will be less than x
            count += row + 1
            col++
        } else {
            row--
        }
    }
    return count
}
```

This approach leverages binary search to efficiently find the k-th smallest element in the sorted matrix by narrowing down the possible values between the minimum and maximum values in the matrix while using a counting method to find the correct placement.

