# [Leetcode 239: Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Approaches
1. [Brute Force](#approach-1-brute-force)
2. [Deque (Double-Ended Queue)](#approach-2-using-deque-double-ended-queue)
3. [Priority Queue (Heap)](#approach-3-using-priority-queue-heap)

---

## Approach 1: Brute Force

### Intuition
The naive approach to solve the problem is to slide a window across the list from the start to the end. For each position of the window, simply compute the maximum value within the current window. This is done by iterating over each element in the window and keeping track of the maximum in a separate variable.

### Steps
1. Initialize an empty list to store results.
2. From `i` from 0 to `len(nums) - k` (inclusive), do the following:
   - Initialize `max_val` to negative infinity.
   - Iterate `j` from `i` to `i + k`, and update `max_val` as `max(nums[j], max_val)`.
   - Append `max_val` to results.
3. Return the results list after iterating through all possible windows.

### Code

```python
def maxSlidingWindow(nums, k):
    n = len(nums)
    if n * k == 0:
        return []
    if k == 1:
        return nums
    
    # Resultant list to store the maximums
    max_list = []
    
    for i in range(n - k + 1):
        # Start with minimum possible integer value
        max_val = float('-inf')
        for j in range(i, i + k):
            # Update max_val within the window
            max_val = max(max_val, nums[j])
        # Append the current maximum to the result
        max_list.append(max_val)
    
    return max_list
```

### Complexity
- **Time complexity**: O((n - k + 1) * k) or O(n * k) in the worst case where n is the length of the array.
- **Space complexity**: O(n - k + 1) which is the space to store the output.

---

## Approach 2: Using Deque (Double-Ended Queue)

### Intuition
The idea is to use a deque to store indices of useful elements for each window. The deque helps in efficiently maintaining the maximum of the current window. Deque maintains elements in decreasing order, which ensures the front of the deque is the largest in the current window, thus achieving an amortized O(1) time complexity for finding the maximum.

### Steps
1. Initialize a deque and result list.
2. Iterate over the elements in `nums`:
   - Remove indices that are out of the current window from the front of the deque.
   - Remove indices whose elements are less than current from the end of the deque because they are useless for the current window.
   - Add the current index to the deque.
   - The front of the deque is the largest element. Append it to the result after at least `k` elements have been processed.
3. Return the results list.

### Code

```python
from collections import deque

def maxSlidingWindow(nums, k):
    deq = deque()
    result = []
    for i in range(len(nums)):
        # Ensure the first element of deque is within the window
        if deq and deq[0] == i - k:
            deq.popleft()
        
        # Maintain elements in decreasing order
        while deq and nums[deq[-1]] < nums[i]:
            deq.pop()
        
        # Add current index to deque
        deq.append(i)
        
        # Append to result when at least 'k' elements have been processed
        if i >= k - 1:
            result.append(nums[deq[0]])
            
    return result
```

### Complexity
- **Time complexity**: O(n), where n is the length of the array. Each element is processed at most twice (once on entering and once on removing from deque).
- **Space complexity**: O(k), where k is the size of the window. The deque stores indices of maximum possible size k.

---

## Approach 3: Using Priority Queue (Heap)

### Intuition
The idea is to use a priority queue (max heap) to maintain the sliding window maximum efficiently. The heap will maintain tuples of negative values along with indices to simulate a max heap (since Python's heapq is a min heap by default).

### Steps
1. Use a max heap to keep track of elements and their indices (use negative values since heapq is min-based).
2. Iterate over the elements:
   - Remove elements from the heap whose indices are out of the current window.
   - Add current value (as negative) and index to the heap.
   - The heap root is the maximum for the current window. Add this to the results list.
3. Return the results list.

### Code

```python
import heapq

def maxSlidingWindow(nums, k):
    if not nums:
        return []
    
    # Initialize heap with elements in a form (negative value, index)
    heap = []
    result = []
    
    for i in range(len(nums)):
        # Remove elements not within the window
        while heap and heap[0][1] <= i - k:
            heapq.heappop(heap)
        
        # Push current value with its index
        heapq.heappush(heap, (-nums[i], i))
        
        # Append current max to the result once the first window is completed
        if i >= k - 1:
            result.append(-heap[0][0])
            
    return result
```

### Complexity
- **Time complexity**: O(n log k), where n is the length of the array. Each insertion and deletion in the heap takes log k time.
- **Space complexity**: O(n), to store the heap and the output.

