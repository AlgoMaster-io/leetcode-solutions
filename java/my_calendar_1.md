# 729. [My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approach 1: Brute Force with List of Intervals

### Solution
```java
// Time Complexity: O(n^2), where n is the number of events booked
// Space Complexity: O(n), where n is the number of events stored
import java.util.ArrayList;
import java.util.List;

public class MyCalendar {

    private List<int[]> calendar;

    public MyCalendar() {
        calendar = new ArrayList<>();
    }

    public boolean book(int start, int end) {
        for (int[] event : calendar) {
            // Check if the new event overlaps with any existing event
            if (start < event[1] && end > event[0]) {
                return false; // Overlap found, booking fails
            }
        }
        calendar.add(new int[]{start, end}); // No overlap, booking succeeds
        return true;
    }
}
```

## Approach 2: TreeMap for Efficient Overlap Check

### Solution
```java
// Time Complexity: O(log n) per booking, where n is the number of events
// Space Complexity: O(n), where n is the number of events stored
import java.util.TreeMap;

public class MyCalendar {

    private TreeMap<Integer, Integer> calendar;

    public MyCalendar() {
        calendar = new TreeMap<>();
    }

    public boolean book(int start, int end) {
        // Get the closest event that starts before the new event
        Integer prev = calendar.floorKey(start);
        // Get the closest event that starts after the new event
        Integer next = calendar.ceilingKey(start);

        // Check for overlap with the previous or next event
        if ((prev == null || calendar.get(prev) <= start) && 
            (next == null || next >= end)) {
            calendar.put(start, end); // No overlap, booking succeeds
            return true;
        }
        return false; // Overlap found, booking fails
    }
}
```