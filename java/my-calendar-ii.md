# [731. My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

## Table of Contents

- [Approach 1: Brute Force with Double Booking Check](#approach-1)
- [Approach 2: Using ArrayList Overlap and Bookings](#approach-2)
- [Approach 3: TreeMap for Optimized Time Complexity](#approach-3)

## Approach 1: Brute Force with Double Booking Check

### Intuition
The brute force approach involves checking if a new event can be added without causing a double booking. This requires maintaining two lists: one for all booked events and another for overlaps of these events. Every new event is checked against these lists, ensuring it doesn't add a triple booking.

### Solution

```java
import java.util.ArrayList;
import java.util.List;

class MyCalendarTwo {
    private List<int[]> bookings;
    private List<int[]> overlaps;

    public MyCalendarTwo() {
        bookings = new ArrayList<>();
        overlaps = new ArrayList<>();
    }

    public boolean book(int start, int end) {
        // Check for any triple booking that would result from this new event
        for (int[] overlap : overlaps) {
            if (start < overlap[1] && end > overlap[0]) {
                return false; // This means there would be a triple booking
            }
        }

        // Update overlaps list with new overlaps caused by the new event
        for (int[] booking : bookings) {
            if (start < booking[1] && end > booking[0]) {
                overlaps.add(new int[]{Math.max(start, booking[0]), Math.min(end, booking[1])});
            }
        }

        // Finally add this event to the list of bookings
        bookings.add(new int[]{start, end});
        return true;
    }
}
```

### Complexity
- **Time Complexity**: \(O(n^2)\), where \(n\) is the number of calls to the `book` method, because each booking may need to check against every other booking for overlap.
- **Space Complexity**: \(O(n)\), to store all bookings and potential overlaps.

## Approach 2: Using ArrayList Overlap and Bookings

### Intuition
An optimization over the previous approach. Instead of blindly checking and updating as in brute force, we focus on preventing any triple bookings by carefully maintaining only necessary overlaps.

### Solution

```java
import java.util.ArrayList;
import java.util.List;

class MyCalendarTwo {
    private List<int[]> bookings;
    private List<int[]> overlaps;

    public MyCalendarTwo() {
        bookings = new ArrayList<>();
        overlaps = new ArrayList<>();
    }

    public boolean book(int start, int end) {
        // Check for any double booking
        for (int[] overlap : overlaps) {
            if (start < overlap[1] && end > overlap[0]) {
                return false;
            }
        }

        // Account for new overlaps caused by new booking
        for (int[] booking : bookings) {
            if (start < booking[1] && end > booking[0]) {
                overlaps.add(new int[]{Math.max(start, booking[0]), Math.min(end, booking[1])});
            }
        }

        // Add current booking to bookings list
        bookings.add(new int[]{start, end});
        return true;
    }
}
```

### Complexity
- **Time Complexity**: \(O(n^2)\), similar to the brute force approach.
- **Space Complexity**: \(O(n)\), to maintain lists of bookings and overlaps.

## Approach 3: TreeMap for Optimized Time Complexity

### Intuition
Using a TreeMap allows us to efficiently track the number of active bookings at any point in time. By incrementing at the start of a booking and decrementing at the end, we can determine double bookings through a sweep-line algorithm.

### Solution

```java
import java.util.Map;
import java.util.TreeMap;

class MyCalendarTwo {
    private TreeMap<Integer, Integer> calendar;

    public MyCalendarTwo() {
        calendar = new TreeMap<>();
    }

    public boolean book(int start, int end) {
        // Increment the count of ongoing events at 'start' and decrement at 'end'
        calendar.put(start, calendar.getOrDefault(start, 0) + 1);
        calendar.put(end, calendar.getOrDefault(end, 0) - 1);

        int activeBookings = 0;
        // Iterate through the times to check active bookings
        for (int delta : calendar.values()) {
            activeBookings += delta;
            if (activeBookings >= 3) {  // Triple booking occurs
                calendar.put(start, calendar.get(start) - 1);
                calendar.put(end, calendar.get(end) + 1);
                return false;
            }
        }
        return true;  // No triple booking, booking is successful
    }
}
```

### Complexity
- **Time Complexity**: \(O(n \log n)\), due to TreeMap operations per booking.
- **Space Complexity**: \(O(n)\), holding start and end times in the map.

Each approach is progressively more efficient, culminating in the TreeMap usage that ensures scalability even with higher numbers of bookings.

