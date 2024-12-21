# [Leetcode 933: Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Approaches:
- [Approach 1: Brute Force Approach using List](#approach-1-brute-force-approach-using-list)
- [Approach 2: Efficient Approach using collections.deque](#approach-2-efficient-approach-using-collections-deque)

## Approach 1: Brute Force Approach using List

### Intuition:
The problem requires us to count the number of recent calls within a window of 3000 milliseconds. A straightforward way is to store every ping in a list and then iterate over the list to count how many pings fall within the desired window. Though simple, this approach can become inefficient due to the repeated iteration over possibly large lists.

### Steps:
1. Maintain a list to keep track of each ping timestamp.
2. For each incoming ping:
    - Append the timestamp to the list.
    - Traverse the list in reverse to count how many timestamps are greater than or equal to `t-3000`.
    - Return the count.

### Code:
```python
class RecentCounter:
    def __init__(self):
        self.pings = []

    def ping(self, t: int) -> int:
        self.pings.append(t)
        # Count how many pings are in the range [t-3000, t]
        return len([time for time in self.pings if time >= t - 3000])
```

### Time Complexity:
- Each call to `ping` takes **O(n)** time in the worst case, where `n` is the number of pings.
- Space complexity is **O(n)** since it stores all the pings.

## Approach 2: Efficient Approach using collections.deque

### Intuition:
The brute force approach is inefficient because we unnecessarily check pings that are too old. To streamline this, we can use a double-ended queue (deque) to keep only relevant pings within the 3000 milliseconds window. Since deques allow for efficient appends and pops from both ends, we can maintain a dynamically sized list of relevant pings.

### Steps:
1. Use a `deque` to store timestamps of pings.
2. For each new ping:
   - Append the timestamp to the deque.
   - Remove timestamps from the front of the deque that are less than `t-3000`.
   - The size of the deque gives the count of recent pings.
   
### Code:
```python
from collections import deque

class RecentCounter:
    def __init__(self):
        self.pings = deque()

    def ping(self, t: int) -> int:
        # Add the current ping time to the deque
        self.pings.append(t)
        # Remove outdated pings
        while self.pings and self.pings[0] < t - 3000:
            self.pings.popleft()
        # The size of the deque represents the number of recent pings
        return len(self.pings)
```

### Time Complexity:
- Each call to `ping` is **O(1)** amortized, since each ping can be added and removed from the deque at most once.
- Space complexity is **O(n)** for storing up to `n` relevant pings.

The second approach using `collections.deque` is significantly more efficient and is the optimal solution for this problem.

