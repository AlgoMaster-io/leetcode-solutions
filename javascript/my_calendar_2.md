# 731. [My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

## Approach: Line Sweeping Using Map

### Solution
```javascript
// Time Complexity: O(n^2), where n is the number of events booked
// Space Complexity: O(n), where n is the number of events stored in the map

class MyCalendarTwo {
    constructor() {
        this.events = new Map();
    }

    book(start, end) {
        // Increment count at the start of the event
        this.events.set(start, (this.events.get(start) || 0) + 1);
        // Decrement count at the end of the event
        this.events.set(end, (this.events.get(end) || 0) - 1);

        let active = 0; // Tracks active bookings at any time
        for (let count of this.events.values()) {
            active += count;
            if (active >= 3) {
                // Triple booking found, revert changes and return false
                this.events.set(start, this.events.get(start) - 1);
                if (this.events.get(start) === 0) {
                    this.events.delete(start);
                }
                this.events.set(end, this.events.get(end) + 1);
                if (this.events.get(end) === 0) {
                    this.events.delete(end);
                }
                return false;
            }
        }

        return true; // No triple booking, booking succeeds
    }
}
```

