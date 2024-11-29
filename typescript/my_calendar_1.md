# 729. [My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approach 1: Brute Force with List of Intervals

### Solution
typescript
```typescript
// Time Complexity: O(n^2), where n is the number of events booked
// Space Complexity: O(n), where n is the number of events stored
class MyCalendar {

    private calendar: Array<[number, number]>;

    constructor() {
        this.calendar = [];
    }

    public book(start: number, end: number): boolean {
        for (const event of this.calendar) {
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
typescript
```typescript
// Time Complexity: O(log n) per booking, where n is the number of events
// Space Complexity: O(n), where n is the number of events stored
class MyCalendar {

    private calendar: Map<number, number>;

    constructor() {
        this.calendar = new Map<number, number>();
    }

    public book(start: number, end: number): boolean {
        // Convert Map to sorted array of keys for floorKey and ceilingKey operations
        const keys = Array.from(this.calendar.keys()).sort((a, b) => a - b);
        
        let prev: number | null = null;
        let next: number | null = null;

        for (let i = 0; i < keys.length; i++) {
            if (keys[i] <= start) {
                prev = keys[i];
            }
            if (keys[i] >= start && !next) {
                next = keys[i];
                break;
            }
        }

        // Check for overlap with the previous or next event
        if ((prev === null || this.calendar.get(prev)! <= start) && 
            (next === null || next >= end)) {
            this.calendar.set(start, end); // No overlap, booking succeeds
            return true;
        }
        return false; // Overlap found, booking fails
    }
}
```

