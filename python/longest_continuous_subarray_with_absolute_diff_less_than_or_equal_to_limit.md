# [1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Approach 1: Sliding Window with Deque

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
from collections import deque

class Solution:
    def longestSubarray(self, nums, limit):
        maxDeque = deque() # Stores indices of max elements in the window
        minDeque = deque() # Stores indices of min elements in the window
        left = 0
        maxLength = 0

        for right in range(len(nums)):
            # Maintain the decreasing order in maxDeque
            while maxDeque and nums[maxDeque[-1]] < nums[right]:
                maxDeque.pop()
            maxDeque.append(right)

            # Maintain the increasing order in minDeque
            while minDeque and nums[minDeque[-1]] > nums[right]:
                minDeque.pop()
            minDeque.append(right)

            # Check if the current window is valid
            while nums[maxDeque[0]] - nums[minDeque[0]] > limit:
                left += 1 # Shrink the window from the left
                # Remove indices out of the window
                if maxDeque[0] < left:
                    maxDeque.popleft()
                if minDeque[0] < left:
                    minDeque.popleft()

            maxLength = max(maxLength, right - left + 1) # Update the max length

        return maxLength
```

## Approach 2: Sliding Window with TreeMap (Using `SortedDict` as an equivalent in Python)

### Solution
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
from sortedcontainers import SortedDict

class Solution:
    def longestSubarray(self, nums, limit):
        map = SortedDict() # Stores elements and their frequencies
        left = 0
        maxLength = 0

        for right in range(len(nums)):
            # Add the current element to the map
            map[nums[right]] = map.get(nums[right], 0) + 1

            # Shrink the window if the condition is violated
            while map.peekitem(-1)[0] - map.peekitem(0)[0] > limit:
                map[nums[left]] -= 1
                if map[nums[left]] == 0:
                    del map[nums[left]]
                left += 1

            maxLength = max(maxLength, right - left + 1) # Update the max length

        return maxLength
```

