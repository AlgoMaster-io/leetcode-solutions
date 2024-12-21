# [Leetcode 378: Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Min-Heap](#approach-2-min-heap)
- [Approach 3: Binary Search on Value](#approach-3-binary-search-on-value)

## Approach 1: Brute Force
### Intuition
The simplest and naive approach is to flatten the matrix and sort it. After sorting, we can directly return the k-th smallest element from this sorted list. This approach leverages the ability to sort an array efficiently but does not take advantage of the matrix's inherently sorted nature along both rows and columns.

### Code
```python
def kthSmallest(matrix, k):
    # Flatten the matrix into a single list
    flat_list = [element for row in matrix for element in row]
    # Sort the flattened list
    flat_list.sort()
    # Return the k-th smallest element (converting from 1-based index to 0-based index)
    return flat_list[k-1]
```

### Time Complexity
- **O(n^2 log(n^2))**, where n is the number of rows or columns in the matrix. This time comes from sorting the flattened list of size n^2.

### Space Complexity
- **O(n^2)**, due to the storage of the flattened list.

## Approach 2: Min-Heap
### Intuition
We can utilize a min-heap (priority queue) to efficiently find the k-th smallest element. The essence of the approach is to iteratively extract the smallest elements from the matrix using the priority queue. Begin by adding all items from the first column to the heap. Extract the smallest element k times. After each extraction, insert the next element from the same row into the heap if available. This approach efficiently narrows down the smallest elements in log time when compared to the size of the heap.

### Code
```python
import heapq

def kthSmallest(matrix, k):
    n = len(matrix)
    # Initialize a min-heap with elements from the first column
    min_heap = [(matrix[i][0], i, 0) for i in range(n)]
    heapq.heapify(min_heap)

    # Extract the minimum element k-1 times
    for _ in range(k-1):
        val, r, c = heapq.heappop(min_heap)
        if c + 1 < n:
            # Push the next element in the same row to the heap
            heapq.heappush(min_heap, (matrix[r][c+1], r, c+1))

    # The top of the heap is the k-th smallest element
    return heapq.heappop(min_heap)[0]
```

### Time Complexity
- **O(k log n)**, where n is the number of rows. Heap operations take log n time. In the worst case, we might need to perform the operation up to k times.

### Space Complexity
- **O(n)**, for storing elements in the heap from n rows.

## Approach 3: Binary Search on Value
### Intuition
Given the matrix is sorted both row-wise and column-wise, we can apply binary search based on the value range. The smallest element will be at the upper left corner, and the largest element at the lower right corner. We can perform binary search for the value until we find the k-th smallest. For each middle value during binary search, we count the number of elements less than or equal to that value by traversing the matrix starting from the bottom-left corner.

### Code
```python
def kthSmallest(matrix, k):
    n = len(matrix)
    
    def count_less_equal(mid):
        count = 0
        row, col = n - 1, 0
        while row >= 0 and col < n:
            if matrix[row][col] <= mid:
                # If the current element is less than or equal to mid,
                # all elements in this row up to this col are less than or equal to mid
                count += row + 1
                col += 1
            else:
                # Move upwards in the same column
                row -= 1
        return count

    left, right = matrix[0][0], matrix[n-1][n-1]
    while left < right:
        mid = (left + right) // 2
        if count_less_equal(mid) < k:
            left = mid + 1
        else:
            right = mid
    return left
```

### Time Complexity
- **O(n log(max-min))**, where max and min are the maximum and minimum elements in the matrix. We binary search over the value range, performing O(n) operations for each search step.

### Space Complexity
- **O(1)**, as we only use a fixed amount of additional space.

