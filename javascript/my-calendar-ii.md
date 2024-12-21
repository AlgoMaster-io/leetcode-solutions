# [Leetcode 731: My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

## Approaches
1. [Naive Approach: Overlapping Check](#naive-approach-overlapping-check)
2. [Optimal Approach: Interval Tree](#optimal-approach-interval-tree)

---

### Naive Approach: Overlapping Check

#### Intuition
The naive approach is to maintain two lists: one for the events that have been booked and another for events that are double booked. When a new event is added, it is compared to all previously booked events to detect overlapping. If a new event overlaps with an already double booked event, it cannot be added. If it overlaps with a single booked event, it is added to the double booked list as needed.

#### Steps
1. Initialize two lists, `calendar` for booked events and `overlap` for double booked events.
2. For every new event:
   - Check overlap with events in `overlap`. If there's an overlap, return false.
   - Check overlap with events in `calendar`. If there's an overlap, add the overlapping interval to `overlap`.
3. If no overlaps were in `overlap`, the new event is added to both `calendar`.

#### Code
```javascript
var MyCalendarTwo = function() {
    // Stores booked events
    this.calendar = [];
    // Stores double-booked sections
    this.overlap = [];
};

/** 
 * @param {number} start 
 * @param {number} end
 * @return {boolean}
 */
MyCalendarTwo.prototype.book = function(start, end) {
    // Check overlap events for triple bookings
    for (const [os, oe] of this.overlap) {
        // There is an overlap if start < oe and end > os
        if (start < oe && end > os) {
            return false; // Triple booking, return false
        }
    }
    
    // Check current bookings and make double bookings
    for (const [cs, ce] of this.calendar) {
        if (start < ce && end > cs) {
            // Calculate the overlap and add it to overlap
            this.overlap.push([Math.max(start, cs), Math.min(end, ce)]);
        }
    }
    
    // Book the event
    this.calendar.push([start, end]);
    return true;
};
```

#### Complexity
- **Time Complexity:** O(N^2) in the worst scenario. For every booking, we check all the overlapping intervals.
- **Space Complexity:** O(N), for storing `calendar` and `overlap`.

---

### Optimal Approach: Interval Tree

#### Intuition
A more optimal approach involves using a balanced tree or similar data structure configured for efficient range queries, specifically segment tree or balanced BST like AVL/Red-Black tree. However, we will simulate these operations using sorted arrays and binary search, taking advantage of sorted properties to optimize the overlap detection and management.

#### Steps
1. Store intervals representing a 'start' and 'end' as events in a timeline.
2. Use a counter to track the number of concurrent events.
3. Traverse the sorted events and calculate active intervals, mark double booked segments.
4. Prevent events that push any segment into three or more concurrent bookings.

#### Code
```javascript
var MyCalendarTwo = function() {
    // Timeline of all events
    this.events = [];
};

/** 
 * @param {number} start 
 * @param {number} end
 * @return {boolean}
 */
MyCalendarTwo.prototype.book = function(start, end) {
    this.events.push([start, 1]); // '1' for a start point
    this.events.push([end, -1]);  // '-1' for an end point
    
    // Sort the events in timeline, sorting by time and type (end < start)
    this.events.sort((a, b) => a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]);
    
    let ongoingBookings = 0;
    
    // Check if there would be any triple bookings
    for (const [_, type] of this.events) {
        ongoingBookings += type;
        if (ongoingBookings >= 3) {
            // Revert the current addition as it creates a third overlap
            this.events.pop();
            this.events.pop();
            return false;
        }
    }
    
    // Booking is valid
    return true;
};
```

#### Complexity
- **Time Complexity:** O(N log N), mainly due to sorting operations.
- **Space Complexity:** O(N), for storing event points.

Each approach balances tradeoffs between ease of implementation and efficiency, especially as the number of bookings grow. The optimal approach efficiently handles the constraints through sorting and traversal, adhering closely to real-time conditions.

