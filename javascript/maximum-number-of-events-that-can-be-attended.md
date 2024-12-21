# [Leetcode 1353: Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/)

## Approaches

1. [Greedy Approach using Sorting](#greedy-approach-using-sorting)
2. [Greedy Approach with Priority Queue (Heap)](#greedy-approach-with-priority-queue)

---

### Greedy Approach using Sorting

#### Intuition

The problem of attending the maximum number of events can be tackled using a greedy approach. The idea is to sort the events based on their end day and try to attend as many events as possible. By attending events that end earlier, it maximizes our opportunities to attend future events.

#### Approach

1. **Sort Events**: First, sort the events by their end day. In case of ties (same end day), sort by start day.
2. **Iterate through Days**: Use a set or boolean array to track which days have been attended.
3. **Attend Events**: For each event, starting from its start day to end day, check if it can be attended (i.e., find the first available day in its duration range). If a day is free, mark it as attended.

#### Code

```javascript
function maxEvents(events) {
    // Sort events by end day, with a secondary sort on start day if end days are the same
    events.sort((a, b) => a[1] - b[1] || a[0] - b[0]);

    let attended = new Set();  // Track the days on which events are attended

    for (let i = 0; i < events.length; i++) {
        let start = events[i][0];
        let end = events[i][1];

        for (let d = start; d <= end; d++) {
            if (!attended.has(d)) {
                attended.add(d);  // Attend the event on this day
                break;  // Move to the next event
            }
        }
    }

    return attended.size;  // Number of unique days attended
}
```

#### Time Complexity

- **Sorting**: \(O(N \log N)\) due to sorting the events.
- **Iterating through events**: \(O(ND)\), where \(D\) is the average duration of events.
- Overall: \(O(N \log N + ND)\)

#### Space Complexity

- \(O(D)\) for tracking attended days.

---

### Greedy Approach with Priority Queue (Heap)

#### Intuition

Using a priority queue (min-heap) enhances our approach by managing the events dynamically. The idea is to attend events by their earliest possible start time, while maintaining those that can still be attended in a min-heap.

#### Approach

1. **Sort Events by Start Day**: First, sort the events by their start day.
2. **Min-Heap for Events**: Use a priority queue to store events by their end day.
3. **Iterate Through Days**: For each day, try to attend as many events as possible from the heap. Remove events from the heap that have already ended.
4. **Push New Events**: Each day, add new events starting that day to the heap.

#### Code

```javascript
function maxEvents(events) {
    // Sort events by start day
    events.sort((a, b) => a[0] - b[0]);

    let maxDay = Math.max(...events.map(event => event[1]));
    let heap = new MinPriorityQueue({ priority: event => event[1] }); // Events sorted by end day
    let eventsIndex = 0;
    let attendedCount = 0;

    for (let day = 1; day <= maxDay; day++) {
        // Add all events starting today
        while (eventsIndex < events.length && events[eventsIndex][0] === day) {
            heap.enqueue(events[eventsIndex]);
            eventsIndex++;
        }

        // Remove all events that have already ended
        while (!heap.isEmpty() && heap.front().element[1] < day) {
            heap.dequeue();
        }

        // Attend the event with the earliest end day
        if (!heap.isEmpty()) {
            heap.dequeue();  // Attend this event
            attendedCount++;
        }
    }

    return attendedCount;
}
```

#### Time Complexity

- **Sorting**: \(O(N \log N)\)
- **Heap Operations**: \(O(N \log N)\)
- Overall: \(O(N \log N)\), as heap insertions and deletions are logarithmic.

#### Space Complexity

- \(O(N)\) due to the events stored in the heap.

With this approach, we achieve a more efficient solution by leveraging a priority queue to dynamically manage attendance opportunities.

