# [Leetcode 480: Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Table of Contents
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach Using Heaps](#optimized-approach-using-heaps)

---

## Brute Force Approach

### Intuition
The brute force approach to finding the median in a sliding window is to extract the current window from the list of numbers, sort it, and then find the median. This approach is straightforward but inefficient due to the need to re-sort the array for every step of the sliding window.

### Approach
1. Loop through the input list with a window of the specified size `k`.
2. For each window, extract the numbers, sort them, and compute the median.
3. Append the median to the result list.

### Python Code
```python
def medianSlidingWindow(nums, k):
    def findMedian(sorted_window):
        n = len(sorted_window)
        if n % 2 == 0:
            return (sorted_window[n // 2 - 1] + sorted_window[n // 2]) / 2
        else:
            return sorted_window[n // 2]

    result = []
    for i in range(len(nums) - k + 1):
        window = sorted(nums[i:i + k])  # Sort the current window
        median = findMedian(window)  # Calculate the median of the window
        result.append(median)  # Add the median to the results list

    return result

```

### Time Complexity
- Sorting the window in each step leads to a time complexity of \(O(n \cdot k \log k)\), where \(n\) is the length of `nums` and \(k\) is the window size.

### Space Complexity
- Storing the sorted window takes \(O(k)\) space.

---

## Optimized Approach Using Heaps

### Intuition
This approach leverages two heaps (a max-heap and a min-heap) to efficiently find the median within a sliding window. By maintaining two heaps, where one half of the numbers (the lower half) is in a max-heap and the other half (the upper half) is in a min-heap, we can quickly access the median.

### Approach
1. Use a max-heap to store the smaller half of numbers and a min-heap for the larger half.
2. Balance these heaps such that their sizes differ at most by 1.
3. As the window slides, maintain the balance by removing and adding elements efficiently.
4. Calculate the median based on the sizes of the heaps.

### Python Code
```python
import heapq

def medianSlidingWindow(nums, k):
    def add_num(num, max_heap, min_heap):
        if not max_heap or num <= -max_heap[0]:
            heapq.heappush(max_heap, -num)
        else:
            heapq.heappush(min_heap, num)

    def balance_heaps(max_heap, min_heap):
        if len(max_heap) > len(min_heap) + 1:
            heapq.heappush(min_heap, -heapq.heappop(max_heap))
        elif len(min_heap) > len(max_heap):
            heapq.heappush(max_heap, -heapq.heappop(min_heap))

    def remove_num(num, max_heap, min_heap):
        try:
            max_heap.remove(-num)
            heapq.heapify(max_heap)
        except ValueError:
            min_heap.remove(num)
            heapq.heapify(min_heap)

    result = []
    max_heap, min_heap = [], []
    
    for i in range(len(nums)):
        add_num(nums[i], max_heap, min_heap)
        if i >= k:
            remove_num(nums[i - k], max_heap, min_heap)
        balance_heaps(max_heap, min_heap)
        
        if i >= k - 1:
            if len(max_heap) > len(min_heap):
                result.append(float(-max_heap[0]))
            else:
                result.append((float(-max_heap[0]) + float(min_heap[0])) / 2)
    
    return result
    
```

### Time Complexity
- Insertion and removal operations on the heaps take \(O(\log k)\). Thus, the overall time complexity is \(O(n \cdot \log k)\).

### Space Complexity
- We use heaps that store at most \(k+1\) elements, leading to a space complexity of \(O(k)\).

