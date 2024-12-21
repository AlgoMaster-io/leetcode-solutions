# [Leetcode 1353: Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/)

## Approaches
- [Approach 1: Sort and Use a Set](#approach-1-sort-and-use-a-set)
- [Approach 2: Greedy with Priority Queue (Optimal)](#approach-2-greedy-with-priority-queue-optimal)

### Approach 1: Sort and Use a Set

#### Intuition
The simplest method is to first sort the list of events based on their end days. Then, iterate over each event to find the earliest day, after the start of the event, that is not occupied, and attend the event on that day.

1. **Sorting**: Sort the events based on their end day.
2. **Attending Events**: Use a set to keep track of days on which events are already attended. For each event, iterate over possible days from the event's start day to its end day, and attend the earliest available day not already occupied.

This method ensures each event is attended on the earliest possible day without conflict.

#### Code
```csharp
public int MaxEvents(int[][] events) {
    // Sort events based on their end date
    Array.Sort(events, (a, b) => a[1].CompareTo(b[1]));
    HashSet<int> attendedDays = new HashSet<int>();

    foreach (var event in events) {
        int start = event[0];
        int end = event[1];
        // Try to attend the event on the earliest possible day
        for (int day = start; day <= end; day++) {
            if (!attendedDays.Contains(day)) {
                // Mark this day as attended
                attendedDays.Add(day);
                break;
            }
        }
    }

    // The number of days we successfully attended events
    return attendedDays.Count;
}
```

#### Time Complexity
- Sorting the events takes \(O(n \log n)\).
- Iterating over events takes \(O(n \cdot d)\) in the worst case, where \(d\) is the range between the start and end of events.
  
Overall, this solution runs in \(O(n \cdot d)\).

#### Space Complexity
- It requires \(O(d)\) space to store the days attended, where \(d\) is the maximum possible day.

### Approach 2: Greedy with Priority Queue (Optimal)

#### Intuition
This more efficient approach uses a priority queue (min-heap) to attend events based on their end times greedily.

1. **Sort Events**: First, sort the events based on their start day. This ensures that we process the earliest starting events first.
2. **Priority Queue Usage**: Utilize a priority queue to dynamically store events' end days which are currently possible to attend.
3. **Iterate Over Days**: For each day, add the end days of ongoing events to the queue and attempt to attend the event which ends the soonest.

This approach optimizes the problem by attending events as late as possible, which leaves room for more future events.

#### Code
```csharp
using System.Collections.Generic;

public int MaxEvents(int[][] events) {
    // Sort events based on their start day
    Array.Sort(events, (a, b) => a[0].CompareTo(b[0]));
    PriorityQueue<int, int> pq = new PriorityQueue<int, int>();
    int i = 0, n = events.Length, day = 0, maxEvents = 0;

    while (i < n || pq.Count > 0) {
        if (pq.Count == 0) {
            // Move the day to the start day of the next event
            day = events[i][0];
        }
        // Add all available events for the current day
        while (i < n && events[i][0] <= day) {
            pq.Enqueue(events[i][1], events[i][1]);
            i++;
        }
        // Attend the event that ends the earliest
        pq.Dequeue();
        maxEvents++;
        day++;

        // Remove events from the queue that we missed
        while (pq.Count > 0 && pq.Peek() < day) {
            pq.Dequeue();
        }
    }

    return maxEvents;
}
```

#### Time Complexity
- The time complexity is \(O(n \log n)\) due to sorting and the priority queue operations where \(n\) is the number of events.

#### Space Complexity
- The space complexity is \(O(n)\) for storing events in the priority queue.

Both approaches offer valid solutions, but the second approach is more efficient and optimal given its reduced time complexity.

