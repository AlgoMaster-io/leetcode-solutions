# [Leetcode 1353: Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/)

## Approaches:
- [Greedy Approach using Sorting](#greedy-approach-using-sorting)
- [Priority Queue (Min-Heap) Approach](#priority-queue-min-heap-approach)

---

## Greedy Approach using Sorting

### Intuition:

The key idea here is to attend as many events as possible by choosing events that finish the earliest. This is because an early completion allows potentially more events to be attended in the future days.

1. **Sort Events:** First, we sort the events by their ending days. If two events end on the same day, sort them by their start day.

2. **Attend Events Sequentially:** We iterate through each possible day and try to attend an event that is scheduled for that day.

    - For each day, check if there is an available event starting on that day. Attend the first available event, and remove it from the list of events.

3. By following this strategy, we maximize the number of events attended since attending the earliest finishing event leaves the maximum room for future events.

### Time Complexity:
- Sorting takes \(O(N \log N)\), where \(N\) is the number of events.
- Iterating through events and possible days results in \(O(N + D)\) where \(D\) is the range of days.

### Space Complexity:
- \(O(1)\), aside from the input storage.

### TypeScript Code:

```typescript
function maxEvents(events: number[][]): number {
    // Sort events by ending day, then by start day if ending days are the same
    events.sort((a, b) => a[1] - b[1] || a[0] - b[0]);

    let attendedEvents = 0;
    const daysSet = new Set<number>(); // To track days on which we can attend events

    for (const [start, end] of events) {
        for (let day = start; day <= end; day++) {
            if (!daysSet.has(day)) { // If no event has been attended on this day
                daysSet.add(day); // Attend event on this day
                attendedEvents++; 
                break; // Move to the next event
            }
        }
    }

    return attendedEvents;
}
```

---

## Priority Queue (Min-Heap) Approach

### Intuition:

The greedy approach works well, but we can optimize by using a priority queue (min-heap) to always attend the events with the earliest end time first. This approach helps efficiently manage overlapping events.

1. **Sort Events by Start Day:** First, we sort events based on their starting day.

2. **Iterate Over Each Day:** Use a min-heap to attend events in order of their ending days.
   - For each day:
       - Add all events that start on this day to the heap.
       - Remove events from the heap that have already expired.
       - Attend the event at the top of the heap (earliest end day), and remove it from the heap.

3. By maintaining a heap of end days, we always prioritize events that finish the earliest, allowing more room for future events.

### Time Complexity:
- Sorting takes \(O(N \log N)\), where \(N\) is the number of events.
- Each event is pushed and popped onto the heap at most once, taking \(O(N \log N)\).

### Space Complexity:
- \(O(N)\) for the heap storage.

### TypeScript Code:

```typescript
function maxEvents(events: number[][]): number {
    // Sort events by start day
    events.sort((a, b) => a[0] - b[0]);
    
    // Priority Queue represented by an array that we'll manually manage
    const minHeap: number[] = [];
    let day = 0;
    let eventIndex = 0;
    let attendedEvents = 0;
    
    while (eventIndex < events.length || minHeap.length > 0) {
        if (minHeap.length === 0 && eventIndex < events.length) {
            // If heap is empty, jump to the next starting event day
            day = Math.max(day, events[eventIndex][0]);
        }

        // Add all events starting on this day or before
        while (eventIndex < events.length && events[eventIndex][0] <= day) {
            const [start, end] = events[eventIndex];
            minHeap.push(end);
            eventIndex++;
        }

        // Manually sort the minHeap (can be done using library functions for efficiency)
        minHeap.sort((a, b) => a - b);

        // Remove all events that have expired
        while (minHeap.length > 0 && minHeap[0] < day) {
            minHeap.shift();
        }

        // Attend the event that ends the earliest and remove it from the heap
        if (minHeap.length > 0) {
            minHeap.shift();
            attendedEvents++;
            day++;
        }
    }
    
    return attendedEvents;
}
```

In both approaches, we are striving to maximize the number of events by making sure we handle overlapping periods efficiently. Using a priority queue allows us more efficient management and better handling of events as they come.

