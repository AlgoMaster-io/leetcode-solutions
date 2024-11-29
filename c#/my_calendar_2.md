# 731. [My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

## Approach: Line Sweeping Using TreeMap

### Solution
c#
```csharp
// Time Complexity: O(n^2), where n is the number of events booked
// Space Complexity: O(n), where n is the number of events stored in the map
using System;
using System.Collections.Generic;

public class MyCalendarTwo {

    private SortedDictionary<int, int> events;

    public MyCalendarTwo() {
        events = new SortedDictionary<int, int>();
    }

    public bool Book(int start, int end) {
        // Increment count at the start of the event
        if (events.ContainsKey(start)) {
            events[start]++;
        } else {
            events[start] = 1;
        }
        
        // Decrement count at the end of the event
        if (events.ContainsKey(end)) {
            events[end]--;
        } else {
            events[end] = -1;
        }

        int active = 0; // Tracks active bookings at any time
        foreach (var count in events.Values) {
            active += count;
            if (active >= 3) {
                // Triple booking found, revert changes and return false
                events[start]--;
                if (events[start] == 0) {
                    events.Remove(start);
                }
                
                events[end]++;
                if (events[end] == 0) {
                    events.Remove(end);
                }
                return false;
            }
        }

        return true; // No triple booking, booking succeeds
    }
}
```

