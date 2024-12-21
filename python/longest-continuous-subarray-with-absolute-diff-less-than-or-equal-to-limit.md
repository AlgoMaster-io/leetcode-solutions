# [Problem 1438: Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with Ordered Dictionary](#approach-2-sliding-window-with-ordered-dictionary)
- [Approach 3: Sliding Window with Two Deques (Optimal)](#approach-3-sliding-window-with-two-deques-optimal)

## Approach 1: Brute Force

### Intuition
The brute force approach involves checking every possible subarray to determine if it satisfies the condition that the difference between the maximum and minimum elements is less than or equal to the limit. We track the maximum length of such subarrays.

### Solution

```python
def longestSubarray(nums, limit):
    max_length = 0
    n = len(nums)

    for start in range(n):
        min_ele = nums[start]
        max_ele = nums[start]
        for end in range(start, n):
            # Update the minimum and maximum for the current subarray
            min_ele = min(min_ele, nums[end])
            max_ele = max(max_ele, nums[end])

            # Check if the current subarray satisfies the condition
            if max_ele - min_ele <= limit:
                max_length = max(max_length, end - start + 1)
            else:
                break  # Break as further increase in end will only increase the difference

    return max_length
```

**Time Complexity:** \(O(n^2)\) - We traverse all possible pairs of indices.  
**Space Complexity:** \(O(1)\) - Only a few variables are used.

## Approach 2: Sliding Window with Ordered Dictionary

### Intuition
The sliding window approach maintains a valid window of elements where the difference between maximum and minimum is within the limit, using an ordered dictionary for efficiency.

### Solution

```python
from collections import OrderedDict

def longestSubarray(nums, limit):
    window = OrderedDict()
    max_length = 0
    left = 0

    for right, num in enumerate(nums):
        window[num] = window.get(num, 0) + 1
        
        while max(window) - min(window) > limit:
            # Decrease the count of the leftmost number and slide window
            window[nums[left]] -= 1
            if window[nums[left]] == 0:
                del window[nums[left]]
            left += 1

        max_length = max(max_length, right - left + 1)

    return max_length
```

**Time Complexity:** \(O(n \log n)\) - In the worst case, due to the operations on the ordered dictionary.  
**Space Complexity:** \(O(n)\) - Due to the ordered dictionary keeping track of elements within the window.

## Approach 3: Sliding Window with Two Deques (Optimal)

### Intuition
Maintain two deques to store indices of the maximum and minimum elements in the current window. This helps efficiently update the window with maximum and minimum operations and check conditions based on the indices stored.

### Solution

```python
from collections import deque

def longestSubarray(nums, limit):
    max_deque = deque()
    min_deque = deque()
    max_length = 0
    left = 0

    for right, num in enumerate(nums):
        # Maintain the deques for max and min elements
        while max_deque and nums[max_deque[-1]] < num:
            max_deque.pop()
        while min_deque and nums[min_deque[-1]] > num:
            min_deque.pop()
        
        max_deque.append(right)
        min_deque.append(right)

        # If the current window does not satisfy the conditions, move the left pointer
        while nums[max_deque[0]] - nums[min_deque[0]] > limit:
            left += 1
            if max_deque[0] < left:
                max_deque.popleft()
            if min_deque[0] < left:
                min_deque.popleft()

        # Update the maximum length of the valid subarray
        max_length = max(max_length, right - left + 1)

    return max_length
```

**Time Complexity:** \(O(n)\) - Since each element is added and removed from the deque once.  
**Space Complexity:** \(O(n)\) - Due to the usage of the deques.

This optimal approach utilizes two deques to smartly manage the window of elements to ensure we are always checking the maximum valid window size for which the condition holds.

