# [Leetcode 729: My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Binary Search with Sorted Intervals](#binary-search-with-sorted-intervals)
3. [Balanced Tree Data Structure](#balanced-tree-data-structure)

### Brute Force Approach

The brute force approach involves maintaining a list of all the bookings. Whenever a new booking request comes, we need to iterate through this list and check for potential overlaps with existing intervals.

#### Intuition:
- For each new booking, simply check if it overlaps with any of the existing bookings.
- This can be done by checking if the new booking's start time is before an existing booking's end time and the new booking's end time is after an existing booking's start time.

#### Steps:
1. Initialize an empty list to store the intervals.
2. For every new booking request [start, end):
   - Traverse through the list, verifying if the requested booking overlaps with any existing booking.
   - If no overlap is found, append the booking to the list.

#### Time Complexity: 
- The time complexity for each booking is \(O(n)\) where \(n\) is the number of bookings since we need to check all prior bookings.

#### Space Complexity: 
- The space complexity is \(O(n)\) to store all the bookings.

```python
class MyCalendar:
    def __init__(self):
        self.calendar = []
    
    def book(self, start: int, end: int) -> bool:
        # Iterating over each existing booking to check for overlaps.
        for s, e in self.calendar:
            # If there is an overlap, return False.
            if max(start, s) < min(end, e):
                return False
        # No overlaps found, booking can be added.
        self.calendar.append((start, end))
        return True
```

### Binary Search with Sorted Intervals

To optimize the time complexity of the booking process, we can maintain a sorted list of intervals and use binary search to quickly locate where the new interval should be placed to determine any possible overlap.

#### Intuition:
- Use the binary search feature to identify the insertion point of the new booking. This reduces the time needed to check for overlaps by focusing only on the nearest intervals.

#### Steps:
1. Use a binary search method to efficiently determine the insertion index of the new booking.
2. Check for overlap with the immediate neighbors (previous and next bookings).

#### Time Complexity:
- The time complexity for each booking is \(O(n)\) in the worst case due to the potential list insertion.
- The search time is \(O(\log n)\) due to binary search, but insertion remains the limiting factor.

#### Space Complexity:
- The space complexity remains \(O(n)\).

```python
from bisect import bisect_left

class MyCalendar:
    def __init__(self):
        self.calendar = []
    
    def book(self, start: int, end: int) -> bool:
        # Find the correct position using binary search.
        i = bisect_left(self.calendar, (start, end))
        
        # Check with the previous event
        if i > 0 and self.calendar[i - 1][1] > start:
            return False
        # Check with the next event
        if i < len(self.calendar) and self.calendar[i][0] < end:
            return False
        
        # Insert the booking in the correct order
        self.calendar.insert(i, (start, end))
        return True
```

### Balanced Tree Data Structure

Using a balanced tree data structure can further enhance efficiency since it maintains ordered elements with fast insertions and deletions, such as AVL Tree or Red-Black Tree (conceptually similar, not shown explicitly).

#### Intuition:
- By employing a tree, we achieve more balance between quick lookup times and insertion times without needing to manually re-sort a list.

#### Steps:
1. Utilize the tree properties for an automatic insertion order to avoid overlapping efficiently.
2. Since Python does not have a built-in balanced tree, we would conceptually use libraries like `SortedDict` from `sortedcontainers`.

#### Time Complexity:
- The time complexity for each booking when using tree data structures is \(O(\log n)\) for both insertions and lookups.

#### Space Complexity:
- The space complexity is \(O(n)\).

Note: Python does not provide direct tree-set functionality, so this method conceptually explains how a balanced tree algorithm would optimize this further.

```python
from sortedcontainers import SortedDict

class MyCalendar:
    def __init__(self):
        self.calendar = SortedDict()
    
    def book(self, start: int, end: int) -> bool:
        # Checks the interval boundaries within the tree correctly.
        idx = self.calendar.bisect_right(start)
        
        # idx is the first element greater than start
        # Look left
        if idx > 0 and self.calendar.items()[idx - 1][1] > start:
            return False
        
        # Check for the right conflict.
        if idx < len(self.calendar) and self.carousel.keys()[idx] < end:
            return False
        
        # If passes both checks, insert the booking.
        self.calendar[start] = end
        return True
```

These approaches gradually optimize from a straightforward list management technique to leveraging the structured properties of data to improve performance.

