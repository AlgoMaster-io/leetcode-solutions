# [933. Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Approach 1: Brute Force

### Solution
python
```python
# Time Complexity: O(n) per request (where n is the number of calls in the time window)
# Space Complexity: O(n)
class RecentCounter:

    def __init__(self):
        self.requests = []

    def ping(self, t: int) -> int:
        self.requests.append(t)  # Add the new request
        count = 0

        # Count all requests in the range [t - 3000, t]
        for time in self.requests:
            if time >= t - 3000:
                count += 1

        return count
```

## Approach 2: Using a Queue (Optimal Solution)

### Solution
python
```python
# Time Complexity: O(1) per request (amortized)
# Space Complexity: O(n)
from collections import deque

class RecentCounter:

    def __init__(self):
        self.queue = deque()

    def ping(self, t: int) -> int:
        self.queue.append(t)  # Add the new request to the queue

        # Remove requests that are outside the range [t - 3000, t]
        while self.queue and self.queue[0] < t - 3000:
            self.queue.popleft()

        return len(self.queue)  # The size of the queue represents the number of valid requests
```


