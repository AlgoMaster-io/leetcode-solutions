[Leetcode 731: My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

## Approaches
- [Approach 1: Brute Force Approach](#approach-1-brute-force-approach)
- [Approach 2: Using a sorted list and line sweeping](#approach-2-using-a-sorted-list-and-line-sweeping)

### Approach 1: Brute Force Approach

#### Intuition:
The basic idea is to keep track of all single bookings and double bookings separately. When we attempt to book a new event, we must ensure that the time interval doesn't overlap with a double booking. We'll also update both the single and double bookings accordingly.

This brute-force approach checks for conflicts using two lists: one to manage single bookings and another to manage overlapping intervals.

#### Detailed Steps:
1. Maintain two lists:
   - `singleBookings`: To store intervals of single bookings.
   - `doubleBookings`: To store intervals that are already double booked.
2. For each new booking request, ensure that the requested interval does not overlap with any interval in `doubleBookings`.
3. If there's no conflict with `doubleBookings`, add potential overlaps with `singleBookings` to `doubleBookings`.
4. Finally, add the current booking to `singleBookings`.

#### C# Code:
```csharp
public class MyCalendarTwo {
    private List<int[]> singleBookings;
    private List<int[]> doubleBookings;

    public MyCalendarTwo() {
        singleBookings = new List<int[]>();
        doubleBookings = new List<int[]>();
    }

    public bool Book(int start, int end) {
        // Check if new event conflicts with any double bookings
        foreach (var doubleBook in doubleBookings) {
            if (start < doubleBook[1] && end > doubleBook[0]) { // Check for overlap
                return false;
            }
        }
        // Add intersection of the new event with single bookings as double bookings
        foreach (var singleBook in singleBookings) {
            if (start < singleBook[1] && end > singleBook[0]) { // Check for overlap
                doubleBookings.Add(new int[] {
                    Math.Max(start, singleBook[0]), 
                    Math.Min(end, singleBook[1])
                });
            }
        }
        // Finally, record the current event as a single booking
        singleBookings.Add(new int[] { start, end });
        return true;
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: O(N^2), where N is the number of events booked. For each new booking, potentially all existing single bookings are checked for overlap.
- **Space Complexity**: O(N), space needed to store the `singleBookings` and `doubleBookings`.

### Approach 2: Using a Sorted List and Line Sweeping

#### Intuition:
Instead of storing intervals directly, this approach uses a line sweeping technique, which involves marking the beginnings and endings of intervals and processing them in sorted order. This allows us to efficiently handle overlapping intervals.

#### Detailed Steps:
1. Utilize a list of pairs to keep track of all event boundaries. Each pair consists of a time and a change in bookings count (+1 for start, -1 for end).
2. Sort these pairs. When processing, simulate adding/removing events by updating a counter.
3. Check when processing each event if the counter exceeds 2, indicating a triple booking attempt.
4. If an event would cause the counter to reach 3, it overlaps with existing double bookings, and the booking should be declined.

#### C# Code:
```csharp
public class MyCalendarTwo {
    private SortedDictionary<int, int> timeMap;

    public MyCalendarTwo() {
        timeMap = new SortedDictionary<int, int>();
    }

    public bool Book(int start, int end) {
        if (!timeMap.ContainsKey(start)) timeMap[start] = 0;
        if (!timeMap.ContainsKey(end)) timeMap[end] = 0;
        // Mark the event's start and end in the map
        timeMap[start]++;
        timeMap[end]--;

        int ongoingEvents = 0;

        foreach (var entry in timeMap) {
            ongoingEvents += entry.Value;
            if (ongoingEvents >= 3) { // Check if it exceeds two concurrent bookings
                // Revert changes if triple booking detected
                timeMap[start]--;
                timeMap[end]++;
                if (timeMap[start] == 0) timeMap.Remove(start);
                if (timeMap[end] == 0) timeMap.Remove(end);
                return false;
            }
        }
        
        return true;
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: O(N log N), due to sorting and updating the map.
- **Space Complexity**: O(N), for maintaining the entries in the map.

