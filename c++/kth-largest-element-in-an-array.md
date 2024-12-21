# [Leetcode 215: Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approaches
1. [Sorting Approach](#sorting-approach)
2. [Min-Heap Approach](#min-heap-approach)
3. [Quickselect Approach (Optimized)](#quickselect-approach-optimized)


### Sorting Approach

**Intuition**: The simplest approach to find the Kth largest element is to sort the entire array in descending order and then pick the element at the K-1 index position.

This approach leverages the power of sorting, making it straightforward but not the most time-efficient.

```cpp
#include <vector>
#include <algorithm>

int findKthLargest(std::vector<int>& nums, int k) {
    // Sort the vector in descending order
    std::sort(nums.begin(), nums.end(), std::greater<int>());
    
    // Return the k-th largest element
    return nums[k - 1];
}
```

**Time Complexity**: O(N log N), due to the sorting operation.

**Space Complexity**: O(1), if the sorting operations can be done in-place.


### Min-Heap Approach

**Intuition**: Instead of sorting the entire array, use a min-heap that can hold the top K largest elements seen so far. By maintaining a min-heap of size K, the smallest element in the heap is always the K-th largest element of the array.

**Steps**:
1. Create a min-heap using a priority queue.
2. Iterate over each element in the array, adding it to the heap.
3. If the size of the heap exceeds K, remove the smallest element (the top of the min-heap).
4. At the end, the root of the min-heap is the Kth largest element.

```cpp
#include <vector>
#include <queue>

int findKthLargest(std::vector<int>& nums, int k) {
    // Min-Heap priority queue
    std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
    
    // Add elements to the min-heap
    for (int num : nums) {
        minHeap.push(num);
        
        // If the heap exceeds size k, remove the smallest element
        if (minHeap.size() > k) {
            minHeap.pop();
        }
    }
    
    // The k-th largest element is at the root of the min-heap
    return minHeap.top();
}
```

**Time Complexity**: O(N log K), since every element when inserted into the heap takes log K time.

**Space Complexity**: O(K), for storing the K elements in the min-heap.


### Quickselect Approach (Optimized)

**Intuition**: Quickselect is a selection algorithm to find the k-th smallest/largest element in an unordered list. It is related to the quicksort algorithm.

**Steps**:
1. Pick a pivot, and partition the array such that elements greater than the pivot are on one side.
2. Recursively partition one of the subarrays (either left or right of the pivot) where the kth element exists.
3. This process is similar to the partitioning in quicksort but only recursing into the portion of the array that contains the Kth largest element.

The advantage of this method is that it does not require the entire array to be sorted, thus potentially reducing the average time complexity.

```cpp
#include <vector>
#include <algorithm>

int partition(std::vector<int>& nums, int low, int high) {
    int pivot = nums[high];
    int i = low;
    for (int j = low; j < high; ++j) {
        if (nums[j] > pivot) {
            std::swap(nums[i], nums[j]);
            i++;
        }
    }
    std::swap(nums[i], nums[high]);
    return i;
}

int quickSelect(std::vector<int>& nums, int low, int high, int k) {
    int pivotIndex = partition(nums, low, high);
    if (pivotIndex == k) {
        return nums[pivotIndex];
    } else if (pivotIndex < k) {
        return quickSelect(nums, pivotIndex + 1, high, k);
    } else {
        return quickSelect(nums, low, pivotIndex - 1, k);
    }
}

int findKthLargest(std::vector<int>& nums, int k) {
    return quickSelect(nums, 0, nums.size() - 1, k - 1);
}
```

**Time Complexity**: O(N) on average, O(N^2) in the worst case (rarely occurs with a good pivot strategy).

**Space Complexity**: O(1), as no additional data structures are used other than recursion stack.

