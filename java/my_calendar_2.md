# 731. [My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

## Approach: Line Sweeping Using TreeMap

### Solution
```java
// Time Complexity: O(n^2), where n is the number of events booked
// Space Complexity: O(n), where n is the number of events stored in the map
import java.util.TreeMap;

public class MyCalendarTwo {

    private TreeMap<Integer, Integer> events;

    public MyCalendarTwo() {
        events = new TreeMap<>();
    }

    public boolean book(int start, int end) {
        // Increment count at the start of the event
        events.put(start, events.getOrDefault(start, 0) + 1);
        // Decrement count at the end of the event
        events.put(end, events.getOrDefault(end, 0) - 1);

        int active = 0; // Tracks active bookings at any time
        for (int count : events.values()) {
            active += count;
            if (active >= 3) {
                // Triple booking found, revert changes and return false
                events.put(start, events.get(start) - 1);
                if (events.get(start) == 0) {
                    events.remove(start);
                }
                events.put(end, events.get(end) + 1);
                if (events.get(end) == 0) {
                    events.remove(end);
                }
                return false;
            }
        }

        return true; // No triple booking, booking succeeds
    }
}
```