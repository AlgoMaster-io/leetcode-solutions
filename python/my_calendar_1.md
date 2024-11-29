# 729. [My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approach 1: Brute Force with List of Intervals

### Solution
python
```python
# Time Complexity: O(n^2), where n is the number of events booked
# Space Complexity: O(n), where n is the number of events stored

class MyCalendar:

    def __init__(self):
        self.calendar = []

    def book(self, start, end):
        for event in self.calendar:
            # Check if the new event overlaps with any existing event
            if start < event[1] and end > event[0]:
                return False # Overlap found, booking fails
        self.calendar.append((start, end)) # No overlap, booking succeeds
        return True
```

## Approach 2: TreeMap for Efficient Overlap Check

### Solution
python
```python
# Time Complexity: O(log n) per booking, where n is the number of events
# Space Complexity: O(n), where n is the number of events stored
from sortedcontainers import SortedDict

class MyCalendar:

    def __init__(self):
        self.calendar = SortedDict()

    def book(self, start, end):
        # Find the closest event that starts before or at the new event
        prev_event_index = self.calendar.bisect_left(start) - 1
        # Find the closest event that starts after the new event
        next_event_index = self.calendar.bisect_right(start)

        # Check for overlap with the previous or next event
        if (prev_event_index == -1 or self.calendar.peekitem(prev_event_index)[1] <= start) and \
           (next_event_index == len(self.calendar) or self.calendar.peekitem(next_event_index)[0] >= end):
            self.calendar[start] = end # No overlap, booking succeeds
            return True
        return False # Overlap found, booking fails
```

