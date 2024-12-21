# [Leetcode 1353: Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Greedy Using a Priority Queue](#approach-2-greedy-using-a-priority-queue)

---

## Approach 1: Brute Force

### Intuition
The brute force approach involves tracking the days individually and checking which events can be attended day-by-day. For each day, check all events to see which event can finish by the current day - if none can, proceed to the next day. This method aims to maximize the number of events attended by linearly scanning available events day-by-day.

### Code
```java
import java.util.Arrays;

class Solution {
    public int maxEvents(int[][] events) {
        // Sort events by their end days
        Arrays.sort(events, (a, b) -> Integer.compare(a[1], b[1]));

        boolean[] daysAttended = new boolean[100001]; // A large enough array to track all possible days
        int maxEvents = 0;

        for (int[] event : events) {
            // For each event, try to attend on the earliest possible day from startDay to endDay
            for (int day = event[0]; day <= event[1]; day++) {
                if (!daysAttended[day]) {
                    daysAttended[day] = true;
                    maxEvents++;
                    break;
                }
            }
        }

        return maxEvents;
    }
}
```

### Time Complexity
- O(n * d) where `n` is the number of events and `d` is the range of days. Typically infeasible for large inputs.

### Space Complexity
- O(d) where `d` is the range of days for marking attended days.

---

## Approach 2: Greedy Using a Priority Queue

### Intuition
The greedy approach utilizes a priority queue to efficiently choose events in a way that maximizes the number of days available for future events. Sort the events by their start date and use a priority queue to keep track of events that can be attended each day - this allows attending events that end the earliest if multiple events are possible on the same day.

### Code
```java
import java.util.Arrays;
import java.util.PriorityQueue;

class Solution {
    public int maxEvents(int[][] events) {
        // Sort by start day
        Arrays.sort(events, (a, b) -> Integer.compare(a[0], b[0]));
        
        PriorityQueue<Integer> eventQueue = new PriorityQueue<>();
        int day = 0;
        int maxEvents = 0;
        int eventIndex = 0;
        int n = events.length;
        
        while (eventIndex < n || !eventQueue.isEmpty()) {
            // Move the day forward
            if (eventQueue.isEmpty()) {
                day = events[eventIndex][0];
            }

            // Add all events starting on the current day into the queue
            while (eventIndex < n && events[eventIndex][0] <= day) {
                eventQueue.offer(events[eventIndex][1]);
                eventIndex++;
            }

            // Remove events that have already ended
            while (!eventQueue.isEmpty() && eventQueue.peek() < day) {
                eventQueue.poll();
            }

            // Attend the event which ends the earliest
            if (!eventQueue.isEmpty()) {
                eventQueue.poll();
                maxEvents++;
            }
            day++; // Move to the next day
        }
        
        return maxEvents;
    }
}
```

### Time Complexity
- O(n log n), where `n` is the number of events due to sorting and priority queue operations.

### Space Complexity
- O(n), where `n` is the number of events for the priority queue.

---

These solutions progressively evolved from brute force towards a more optimal greedy algorithm, enabling efficient maximization of attended events while respecting all constraints provided by the timeline of each event.

