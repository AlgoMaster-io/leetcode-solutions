# 731. [My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

## Approach: Line Sweeping Using SortedDict

### Solution
python
```python
# Time Complexity: O(n^2), where n is the number of events booked
# Space Complexity: O(n), where n is the number of events stored in the map
from sortedcontainers import SortedDict

class MyCalendarTwo:
    def __init__(self):
        self.events = SortedDict()

    def book(self, start: int, end: int) -> bool:
        # Increment count at the start of the event
        self.events[start] = self.events.get(start, 0) + 1
        # Decrement count at the end of the event
        self.events[end] = self.events.get(end, 0) - 1

        active = 0  # Tracks active bookings at any time
        for count in self.events.values():
            active += count
            if active >= 3:
                # Triple booking found, revert changes and return false
                self.events[start] -= 1
                if self.events[start] == 0:
                    del self.events[start]
                self.events[end] += 1
                if self.events[end] == 0:
                    del self.events[end]
                return False

        return True  # No triple booking, booking succeeds
```

