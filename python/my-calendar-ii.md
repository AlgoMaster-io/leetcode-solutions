# [731. My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

## Approaches
1. [Brute Force Approach with Event Overlap Checking](#approach-1-brute-force-approach-with-event-overlap-checking)
2. [Efficient Interval Tree Approach](#approach-2-efficient-interval-tree-approach)

### Approach 1: Brute Force Approach with Event Overlap Checking

**Intuition**:  
The problem requires checking for three types of overlaps: no overlap, single overlap (valid), and double overlap (invalid). We will store events in a list and for each new event, check if it overlaps with existing ones. If it leads to a double booking (an event overlaps with two existing ones), we reject the new event.

This approach checks all existing events to determine if the new one can be scheduled, and involves checking if adding this new event will overlap with the overlaps already present, leading to a triple booking which is not allowed.

#### Code:

```python
class MyCalendarTwo:
    def __init__(self):
        self.calendar = []
        self.overlaps = []

    def book(self, start: int, end: int) -> bool:
        # Check if the new event causes a triple booking
        for s, e in self.overlaps:
            if start < e and end > s:
                # If overlap is found in overlaps list
                return False

        # Check overlaps with existing calendar events
        for s, e in self.calendar:
            if start < e and end > s:
                # If overlap is found, record this overlap
                self.overlaps.append((max(start, s), min(end, e)))

        # If no triple booking, then add the event
        self.calendar.append((start, end))
        return True
```

**Time Complexity**: \(O(N^2)\), where \(N\) is the number of events, because in the worst case, we check all pairs of calendar events.

**Space Complexity**: \(O(N)\) for storing events and overlaps.

### Approach 2: Efficient Interval Tree Approach

**Intuition**:  
Instead of just maintaining a list of overlapped events, we could use a data structure that helps us check and insert events more efficiently. An interval tree (or similar data structure) will reduce the number of operations required to check for conflicts between events.

To add an event, it won't be confirmed until we ensure that no interval overlaps more than once. Overlapping intervals need to be recorded so we can track any potential double bookings.

#### Code:

```python
from sortedcontainers import SortedDict

class MyCalendarTwo:
    def __init__(self):
        self.bookings = SortedDict()
        
    def book(self, start: int, end: int) -> bool:
        # Increment at start, decrement at end
        self.bookings[start] = self.bookings.get(start, 0) + 1
        self.bookings[end] = self.bookings.get(end, 0) - 1
        
        # Keep track of ongoing bookings
        ongoing = 0
        # Traverse the timeline
        for count in self.bookings.values():
            ongoing += count
            if ongoing > 2:
                # Revert changes if a triple booking detected
                self.bookings[start] -= 1
                self.bookings[end] += 1
                if self.bookings[start] == 0:
                    del self.bookings[start]
                if self.bookings[end] == 0:
                    del self.bookings[end]
                return False
        
        return True
```

**Time Complexity**: \(O(N \log N)\), due to the insertion and update operations in the sorted dictionary.

**Space Complexity**: \(O(N)\), for storing the sorted events.

Both approaches aim to manage a calendar with events where a single booking is preferred, and at most, double booking is permitted. The second approach optimizes the checking process via interval management, suitable for a large number of operations where efficiency is crucial.

