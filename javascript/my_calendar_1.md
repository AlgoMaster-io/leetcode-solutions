# 729. [My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approach 1: Brute Force with List of Intervals

### Solution
```javascript
// Time Complexity: O(n^2), where n is the number of events booked
// Space Complexity: O(n), where n is the number of events stored

class MyCalendar {
    constructor() {
        this.calendar = [];
    }

    book(start, end) {
        for (let event of this.calendar) {
            // Check if the new event overlaps with any existing event
            if (start < event[1] && end > event[0]) {
                return false; // Overlap found, booking fails
            }
        }
        this.calendar.push([start, end]); // No overlap, booking succeeds
        return true;
    }
}
```

## Approach 2: TreeMap for Efficient Overlap Check

### Solution
```javascript
// Time Complexity: O(log n) per booking, where n is the number of events
// Space Complexity: O(n), where n is the number of events stored

class MyCalendar {
    constructor() {
        this.calendar = new Map();
    }

    book(start, end) {
        // Convert Map to array to find the previous and next events surrounding the new event
        let events = Array.from(this.calendar.entries());
        
        let prev = null;
        let next = null;

        for (let [s, e] of events) {
            if (s < start) {
                if (prev === null || s > prev[0]) prev = [s, e];
            }
            if (s >= start) {
                if (next === null || s < next[0]) {
                    next = [s, e];
                    break;
                }
            }
        }

        // Check for overlap with the previous or next event
        if ((prev === null || prev[1] <= start) && 
            (next === null || next[0] >= end)) {
            this.calendar.set(start, end); // No overlap, booking succeeds
            return true;
        }
        return false; // Overlap found, booking fails
    }
}
```

Note: JavaScript does not have a built-in `TreeMap` equivalent. A `Map` is used with manual iteration to check for overlaps, which may not be as efficient as the Java TreeMap-based solution.

