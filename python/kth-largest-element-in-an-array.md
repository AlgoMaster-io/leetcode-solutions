# [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approaches:
1. [Brute Force: Sort and Retrieve](#brute-force-sort-and-retrieve)
2. [Using Max Heap](#using-max-heap)
3. [Quick Select](#quick-select)

---

### 1. Brute Force: Sort and Retrieve

#### Intuition:
The simplest approach to finding the kth largest element in an unsorted array is to sort the array first. Once sorted in descending order, the element at the (k-1)th index will be the kth largest because arrays are 0-indexed.

#### Steps:
1. Sort the array in descending order.
2. Retrieve the element at the (k-1) index.

#### Code:
```python
def findKthLargest(nums, k):
    # Sort the array in descending order
    nums.sort(reverse=True)
    # Return the kth largest element, i.e., element at (k-1) index
    return nums[k - 1]
```

#### Complexity:
- **Time Complexity:** `O(n log n)` - Sorting the array takes `O(n log n)`.
- **Space Complexity:** `O(1)` - Sorting is done in place, not using extra space besides input.

---

### 2. Using Max Heap

#### Intuition:
Use a max heap to efficiently retrieve the largest elements. We can push all elements into a max heap and then extract the maximum element k times to get the kth largest.

#### Steps:
1. Build a max heap from the array.
2. Pop the root of the max heap k times.

#### Code:
```python
import heapq

def findKthLargest(nums, k):
    # Convert the nums array into a max heap
    # We use negative numbers because Python's heapq is a min-heap
    max_heap = [-n for n in nums]
    heapq.heapify(max_heap)
    
    # Extract max k times
    for _ in range(k):
        kth_largest = -heapq.heappop(max_heap)
    
    return kth_largest
```

#### Complexity:
- **Time Complexity:** `O(n + k log n)` - Building the heap takes `O(n)` and extracting k elements takes `O(k log n)`.
- **Space Complexity:** `O(n)` - Due to storing all elements in the heap.

---

### 3. Quick Select

#### Intuition:
Quick Select is an optimized selection algorithm to find the kth smallest/largest element without fully sorting the array. It uses a similar approach to QuickSort, selecting a pivot, and deciding recursively which side of the pivot to look for the kth largest.

#### Steps:
1. Choose a pivot.
2. Partition the array around the pivot, placing elements less than the pivot on one side and greater on the other.
3. Recursively narrow down the side of interest until the pivot is the kth largest.

#### Code:
```python
import random

def findKthLargest(nums, k):
    # Function to perform partition
    def partition(left, right, pivot_index):
        pivot_value = nums[pivot_index]
        # Move pivot to the end
        nums[pivot_index], nums[right] = nums[right], nums[pivot_index]
        store_index = left
        # Move all smaller elements to the left
        for i in range(left, right):
            if nums[i] > pivot_value:
                nums[store_index], nums[i] = nums[i], nums[store_index]
                store_index += 1
        # Move pivot to its final position
        nums[right], nums[store_index] = nums[store_index], nums[right]
        return store_index
    
    # Recursively perform the partition
    def select(left, right, k_smallest):
        if left == right: # If the list contains only one element
            return nums[left] 
        # Select a random pivot_index between left and right
        pivot_index = random.randint(left, right)
        pivot_index = partition(left, right, pivot_index)
        # The pivot is in its final sorted position
        if k_smallest == pivot_index:
            return nums[k_smallest]
        elif k_smallest < pivot_index:
            return select(left, pivot_index - 1, k_smallest)
        else:
            return select(pivot_index + 1, right, k_smallest)

    # kth largest is the Index of k-th smallest in a zero indexed array, so we need `len(nums) - k`
    return select(0, len(nums) - 1, len(nums) - k)
```

#### Complexity:
- **Time Complexity:** `O(n)` on average. Worst case `O(n^2)`, but improved with random pivot selection.
- **Space Complexity:** `O(1)` - It's in-place, but the stack can grow up to `O(n)` due to recursion.

