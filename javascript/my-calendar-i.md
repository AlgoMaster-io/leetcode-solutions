[Leetcode Problem 729: My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approaches:

- [Approach 1: Brute Force Using Simple Array](#approach-1-brute-force-using-simple-array)
- [Approach 2: Optimized Approach Using Balanced BST](#approach-2-optimized-approach-using-balanced-bst)

---

### Approach 1: Brute Force Using Simple Array

#### Intuition:

The task is to implement a calendar where we can book appointments without overlaps. A straightforward approach is to keep a list of all booked intervals and, on each new booking request, verify it doesn't overlap with any already booked intervals.

#### Steps:

1. Use an array to maintain all booked intervals.
2. For each new booking request, iterate through the booked intervals to check for overlap.
3. If an overlap is found, return false. If no overlaps, add the new interval to the list and return true.

#### Code:

```javascript
class MyCalendar {
    constructor() {
        this.bookings = []; // Array to store booked intervals
    }

    book(start, end) {
        // Check for overlap with each booked interval
        for (let [s, e] of this.bookings) {
            // An overlap exists if the start of the new interval is less than
            // the end of an existing interval and the end is greater than the start
            if (start < e && end > s) {
                return false;
            }
        }
        // If no overlap, add the current booking to the list
        this.bookings.push([start, end]);
        return true;
    }
}
```

#### Time Complexity:

- The time complexity is O(n) where n is the number of booked intervals. Each booking operation involves checking every existing interval for overlap.

#### Space Complexity:

- O(n), where n is the number of bookings as we store each booking in the array.

---

### Approach 2: Optimized Approach Using Balanced BST

#### Intuition:

Using a balanced binary search tree (BST) can improve efficiency since checking for overlap can be performed faster. With this approach, each node contains intervals, and the tree maintains a sorted structure that allows binary search capabilities.

#### Steps:

1. Utilize a self-balancing BST to keep the intervals sorted.
2. On each booking attempt, perform a search operation similar to a binary search to find a suitable position for the interval.
3. Validate whether inserting the interval maintains the properties of the calendar (i.e., no overlaps). If yes, perform insertion.

#### Code:

```javascript
class MyCalendar {
    constructor() {
        this.intervals = []; // We'll mimic a balanced BST using an array with sorted intervals.
    }

    book(start, end) {
        // Perform binary search to find the correct position
        let left = 0, right = this.intervals.length;
        
        while (left < right) {
            let mid = Math.floor((left + right) / 2);
            if (this.intervals[mid][0] >= end) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        
        if (left > 0 && this.intervals[left - 1][1] > start) {
            return false;
        } 
        
        this.intervals.splice(left, 0, [start, end]);
        return true;
    }
}
```

#### Time Complexity:

- The time complexity is O(log n) due to the binary search for a suitable position, followed by O(n) for the insertion operation.

#### Space Complexity:

- O(n), where n is the number of bookings, as each booking is stored.

These approaches offer straightforward and optimized solutions to the My Calendar I problem, leveraging basic data structures and algorithm techniques.

