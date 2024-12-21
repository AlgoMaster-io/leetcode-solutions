# [Leetcode 731: My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Using Intervals Tree / Balanced Tree Approach](#using-intervals-tree--balanced-tree-approach)

## Brute Force Approach

### Intuition
The fundamental idea behind My Calendar II is to track single and double bookings efficiently. A brute force approach to handle booking requests:
- Maintain a list of single bookings and a list of double bookings.
- For every new booking request, check if it causes a triple booking in the context of existing double bookings. If it does, reject the booking.
- Otherwise, update the lists of single and double bookings appropriately.

### Plan
1. Maintain two lists: one for single bookings and one for double bookings.
2. For each new booking request, iterate over the list of double bookings to check for conflicts.
3. If a conflict is found with any double booking, this indicates a triple booking, and the request is rejected.
4. If there are no triple bookings, iterate over the single bookings to identify overlaps and create new double bookings accordingly.
5. Insert the new single booking.

### Code

```go
type MyCalendarTwo struct {
    singleBookings [][2]int
    doubleBookings [][2]int
}

func Constructor() MyCalendarTwo {
    return MyCalendarTwo{}
}

func (this *MyCalendarTwo) Book(start int, end int) bool {
    // Check against double bookings if this new booking overlaps creating a triple booking
    for _, db := range this.doubleBookings {
        if start < db[1] && end > db[0] {
            return false // Overlapping a double booking causes a triple booking
        }
    }
    
    // Check existing single bookings to create/update double bookings
    for _, sb := range this.singleBookings {
        if start < sb[1] && end > sb[0] { // Overlap condition with a single booking
            this.doubleBookings = append(this.doubleBookings, [2]int{max(start, sb[0]), min(end, sb[1])})
        }
    }
    
    // Add the new booking to single bookings
    this.singleBookings = append(this.singleBookings, [2]int{start, end})
    return true
}

// Helper functions to find max and min
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

### Complexity Analysis
- **Time Complexity**: `O(n^2)`, where `n` is the number of bookings. In the worst case, we compare each new booking with all previous single and double bookings.
- **Space Complexity**: `O(n)`, storing all single and double bookings.

## Using Intervals Tree / Balanced Tree Approach

### Intuition
To optimize, use a balanced tree or interval tree to efficiently manage bookings. The approach involves managing start and end events separately, leveraging efficient data structures to manage these events and extract bookings in a sorted manner.

### Plan
1. Use a map to manage start and end events of intervals.
2. Iterate over all events in a timeline sorted by time - adding 1 for a start, subtracting 1 for an end.
3. Track the current room count, ensuring it never exceeds two.

### Code

```go
import "sort"

type MyCalendarTwo struct {
    events map[int]int
}

func Constructor() MyCalendarTwo {
    return MyCalendarTwo{events: make(map[int]int)}
}

func (this *MyCalendarTwo) Book(start int, end int) bool {
    // Mark start and end times
    this.events[start]++
    this.events[end]--

    // Sort the events based on time
    eventTimes := make([]int, 0, len(this.events))
    for time := range this.events {
        eventTimes = append(eventTimes, time)
    }
    sort.Ints(eventTimes)
    
    currentBookings := 0
    for _, time := range eventTimes {
        currentBookings += this.events[time]
        if currentBookings >= 3 {
            // Revert changes
            this.events[start]--
            this.events[end]++
            return false
        }
    }
    return true
}
```

### Complexity Analysis
- **Time Complexity**: `O(n log n)`, as sorting is required every time a new booking is processed, where `n` is the number of unique times.
- **Space Complexity**: `O(n)`, storing the unique start and end times.

