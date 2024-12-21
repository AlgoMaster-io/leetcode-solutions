# [Leetcode 1353: Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/)

## Approaches:
1. [Basic Greedy Approach](#basic-greedy-approach)
2. [Optimized Greedy with Min-Heap](#optimized-greedy-with-min-heap)

## Basic Greedy Approach

### Intuition:
This approach starts by sorting the events based on their end times. The idea is to always attend the earliest finishing event which prevents missing out on further events. By selecting an event with the earliest end time that's still available, we maximize the number of events we can attend.

### Steps:
1. Sort the events by their end days.
2. For each day, attempt to attend an available event that finishes on or after the current day.
3. Track attended events to avoid double counting.
  
### Implementation:
```python
def maxEvents(events):
    # Sort events by their end day
    events.sort(key=lambda x: x[1])
    
    # Set to remember visited days
    attended_days = set()
    max_count = 0
    
    for start, end in events:
        # Attend event on the first available day within its range
        for day in range(start, end + 1):
            if day not in attended_days:
                attended_days.add(day)
                max_count += 1
                break
    
    return max_count

# Example usage
# events = [[1,2],[2,3],[3,4]]
# print(maxEvents(events)) # Output: 3
```

### Time and Space Complexity:
- **Time Complexity**: O(NlogN + D*N), where N is the number of events and D is the day range.
- **Space Complexity**: O(D) for storing attended days.

## Optimized Greedy with Min-Heap

### Intuition:
Use a min-heap to efficiently track the events' end days such that at any given day, the heap contains the end days of all active events that can be attended. By always selecting the event which ends the earliest (min-heap), we ensure we attend the most possible events.

### Steps:
1. Sort events by their start day.
2. Iterate through each day, pushing the end days of currently available events onto a min-heap.
3. Each day, attend the event with the earliest end day that is still active.
4. Remove events from heap that have already ended before the current day.
  
### Implementation:
```python
import heapq

def maxEvents(events):
    # Sort events based on their start day (and separately end day doctrine applies via min-heap)
    events.sort(key=lambda x: x[0])
    
    current_day = 0
    event_index = 0
    max_count = 0
    min_heap = []
    
    while event_index < len(events) or min_heap:
        if not min_heap:
            # Move to the next event's start day if no events could be attended
            current_day = events[event_index][0]
        
        # Push all events starting today or earlier into the heap 
        while event_index < len(events) and events[event_index][0] <= current_day:
            heapq.heappush(min_heap, events[event_index][1])
            event_index += 1
        
        # Attend the event ending the earliest
        heapq.heappop(min_heap)
        max_count += 1
        current_day += 1
        
        # Remove events that have ended in the past
        while min_heap and min_heap[0] < current_day:
            heapq.heappop(min_heap)
    
    return max_count

# Example usage
# events = [[1,2],[2,3],[3,4]]
# print(maxEvents(events)) # Output: 3
```

### Time and Space Complexity:
- **Time Complexity**: O(NlogN), for sorting events and processing heap operations.
- **Space Complexity**: O(N), for the heap storage.

