# 729. [My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approach 1: Brute Force with List of Intervals

### Solution
```csharp
// Time Complexity: O(n^2), where n is the number of events booked
// Space Complexity: O(n), where n is the number of events stored
using System.Collections.Generic;

public class MyCalendar {

    private List<int[]> calendar;

    public MyCalendar() {
        calendar = new List<int[]>();
    }

    public bool Book(int start, int end) {
        foreach (var eventInterval in calendar) {
            // Check if the new event overlaps with any existing event
            if (start < eventInterval[1] && end > eventInterval[0]) {
                return false; // Overlap found, booking fails
            }
        }
        calendar.Add(new int[] {start, end}); // No overlap, booking succeeds
        return true;
    }
}
```

## Approach 2: TreeMap for Efficient Overlap Check

### Solution
```csharp
// Time Complexity: O(log n) per booking, where n is the number of events
// Space Complexity: O(n), where n is the number of events stored
using System;
using System.Collections.Generic;

public class MyCalendar {

    private SortedDictionary<int, int> calendar;

    public MyCalendar() {
        calendar = new SortedDictionary<int, int>();
    }

    public bool Book(int start, int end) {
        // Get the closest event that starts before the new event
        if (calendar.TryGetValue(start, out _)) return false; // Start already exists
        var prev = new KeyValuePair<int, int>();
        var next = new KeyValuePair<int, int>();

        foreach (var kv in calendar) {
            if (kv.Key < start) prev = kv;
            if (kv.Key >= start) {
                next = kv;
                break;
            }
        }

        // Check for overlap with the previous or next event
        if ((prev.Key == 0 || prev.Value <= start) &&
            (next.Key == 0 || next.Key >= end)) {
            calendar[start] = end; // No overlap, booking succeeds
            return true;
        }
        return false; // Overlap found, booking fails
    }
}
```

