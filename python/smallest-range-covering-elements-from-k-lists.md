# [Leetcode 632: Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/)

## Approaches

1. [Basic Approach: Use Brute Force and Check Every Possible Range](#basic-approach)
2. [Optimal Approach: Use a Min-Heap to Find the Smallest Range Efficiently](#optimal-approach)

---

## Basic Approach: Use Brute Force and Check Every Possible Range

### Intuition
The basic idea behind the brute force approach is to generate all possible ranges that include at least one element from each list. This involves computing the minimum and maximum of every possible combination of elements taken from each respective list and then updating the smallest range found.

### Code

```python
def smallestRange(lists):
    # Flatten the lists into pairs of (number, index)
    import itertools
    elements = sorted((num, idx) for idx, lst in enumerate(lists) for num in lst)
    
    # Initialize the smallest range to an infinitely large range
    min_range = -float('inf'), float('inf')
    count = {}
    left = 0
    
    # Explore all possible ranges using a sliding window
    for right in range(len(elements)):
        # Increment the count for the current list index
        num, idx = elements[right]
        count[idx] = count.get(idx, 0) + 1
        
        # Check if we have all the lists within the current range
        while len(count) == len(lists):
            # Update the minimum range found so far
            current_range = elements[right][0] - elements[left][0]
            if current_range < min_range[1] - min_range[0]:
                min_range = elements[left][0], elements[right][0]
                
            # Decrement the count for the list starting the range
            count[elements[left][1]] -= 1
            if count[elements[left][1]] == 0:
                del count[elements[left][1]]
            left += 1

    return min_range

# Example Usage:
# lists = [[4, 10, 15, 24, 26], [0, 9, 12, 20], [5, 18, 22, 30]]
# print(smallestRange(lists)) # Expect to get (20, 24)
```

### Complexity
- **Time Complexity**: \(O(n \log n + n \times k)\), where \(n\) is the total number of elements and \(k\) is the number of lists.
- **Space Complexity**: \(O(n + k)\), for storing the flattened list (`elements`) and the counting dictionary (`count`).

---

## Optimal Approach: Use a Min-Heap to Find the Smallest Range Efficiently

### Intuition
The optimal approach involves using a min-heap to efficiently keep track of the smallest current element and iterating to find a better range. It reduces the problem to a more trackable one, where we keep a window of valid elements from each list and minimize the range by pulling the smallest element and pushing the next element from that list.

### Code

```python
import heapq

def smallestRange(lists):
    # Initialize the min-heap
    heap = [(lst[0], i, 0) for i, lst in enumerate(lists)]
    heapq.heapify(heap)
    
    # Determine the initial maximum value from the first elements of each list
    max_val = max(lst[0] for lst in lists)
    min_range = -float('inf'), float('inf')
    
    while heap:
        # Extract the smallest element from the heap
        min_val, from_list, index = heapq.heappop(heap)

        # Update the min_range if a smaller one is found
        if max_val - min_val < min_range[1] - min_range[0]:
            min_range = min_val, max_val

        # Move to next element in the current list
        if index + 1 < len(lists[from_list]):
            next_val = lists[from_list][index + 1]
            heapq.heappush(heap, (next_val, from_list, index + 1))
            # Update the maximum value
            max_val = max(max_val, next_val)
        else:
            # One of the lists has been exhausted, optimal range is found
            break
            
    return min_range

# Example Usage:
# lists = [[4, 10, 15, 24, 26], [0, 9, 12, 20], [5, 18, 22, 30]]
# print(smallestRange(lists)) # Expect to get (20, 24)
```

### Complexity
- **Time Complexity**: \(O(n \log k)\), where \(n\) is the total number of elements in all lists combined and \(k\) is the number of lists. This complexity arises because each element is pushed and popped from the heap at most once.
- **Space Complexity**: \(O(k)\), since the heap can store at most one element from each list.

