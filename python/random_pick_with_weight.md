# 528. [Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approach 1: Linear Search for Picking (Alternative to Binary Search)

### Solution
```python
# Time Complexity: O(n) for initialization, O(n) for each pick
# Space Complexity: O(n)
import random

class Solution:
    def __init__(self, w: List[int]):
        self.prefix_sums = [0] * len(w)
        self.prefix_sums[0] = w[0]

        # Build the prefix sum array
        for i in range(1, len(w)):
            self.prefix_sums[i] = self.prefix_sums[i - 1] + w[i]

    def pickIndex(self) -> int:
        # Generate a random number between 1 and the total sum
        target = random.randint(1, self.prefix_sums[-1])

        # Perform linear search to find the target index
        for i, prefix_sum in enumerate(self.prefix_sums):
            if target <= prefix_sum:
                return i # Return the first index with a prefix sum >= target

        return -1 # Should not reach here
```

## Approach 2: Prefix Sum Array and Binary Search

### Solution
```python
# Time Complexity: O(n) for initialization, O(log n) for each pick
# Space Complexity: O(n)
import random
import bisect

class Solution:
    def __init__(self, w: List[int]):
        self.prefix_sums = [0] * len(w)
        self.prefix_sums[0] = w[0]

        # Build the prefix sum array
        for i in range(1, len(w)):
            self.prefix_sums[i] = self.prefix_sums[i - 1] + w[i]

    def pickIndex(self) -> int:
        # Generate a random number between 1 and the total sum
        target = random.randint(1, self.prefix_sums[-1])

        # Use binary search to find the target index
        index = bisect.bisect_left(self.prefix_sums, target)
        return index # Return the index corresponding to the random number
```

