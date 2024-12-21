# [Leetcode 729: My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Using TreeMap](#optimized-using-treemap)

### Brute Force Approach

#### Intuition
The brute force approach involves storing every booked interval and iterating through the list whenever a new booking request comes in. This approach checks for conflicts by iterating through each previously booked interval and ensuring no overlap.

#### Code
```java
import java.util.ArrayList;
import java.util.List;

class MyCalendar {
    // We will keep a list to store the intervals
    private List<int[]> calendar;

    public MyCalendar() {
        // Initialize the list
        calendar = new ArrayList<>();
    }
    
    public boolean book(int start, int end) {
        // Iterate through each booked interval
        for (int[] interval : calendar) {
            // If there is an overlap, return false
            if (interval[0] < end && start < interval[1]) {
                return false;
            }
        }
        // If no overlaps, add the new booking and return true
        calendar.add(new int[]{start, end});
        return true;
    }
}

```
#### Time and Space Complexity
- **Time Complexity:** O(n), where n is the number of bookings so far. In the worst case, for each new booking, we are checking against all previously stored intervals.
- **Space Complexity:** O(n) due to storing all booked intervals.

---

### Optimized Using TreeMap

#### Intuition
To reduce the time complexity for checking overlaps, we can use a TreeMap. The TreeMap data structure maintains the sorted order of starts, which allows faster lookups. We can utilize its floor and ceiling functions to efficiently check for overlaps with neighboring intervals.

#### Code
```java
import java.util.TreeMap;

class MyCalendar {
    // TreeMap to store the intervals with key as the start time and value as end time
    private TreeMap<Integer, Integer> calendar;

    public MyCalendar() {
        // Initialize the TreeMap
        calendar = new TreeMap<>();
    }
    
    public boolean book(int start, int end) {
        // Find the entry with largest key less than or equal to the start of the new interval
        Integer prev = calendar.floorKey(start);
        // Find the entry with the smallest key greater than the start
        Integer next = calendar.ceilingKey(start);

        // If the new interval overlaps with previous or next interval, return false
        if ((prev != null && calendar.get(prev) > start) || (next != null && next < end)) {
            return false;
        }
        
        // Otherwise, add the new interval
        calendar.put(start, end);
        return true;
    }
}
```
#### Time and Space Complexity
- **Time Complexity:** O(log n) due to the TreeMap operations (floor, ceiling, and put).
- **Space Complexity:** O(n) for storing the intervals in the TreeMap.

Using the TreeMap significantly improves upon the basic approach by reducing the lookup time for overlap checks using its natural ordering properties.

