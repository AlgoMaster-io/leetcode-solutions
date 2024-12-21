# [LeetCode 729: My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Navigation
- [Approach 1: Brute Force using List](#approach-1-brute-force-using-list)
- [Approach 2: Binary Search Tree for Efficient Lookup](#approach-2-binary-search-tree-for-efficient-lookup)

## Approach 1: Brute Force using List

### Intuition
The simplest way to solve the problem is to maintain a list of tuple booking intervals. Every time a new booking is requested, iterate through the current bookings to check if there's an overlap with the new interval. 

An overlap exists if the start time of the new interval is less than the end time of an already booked interval and the end time of the new interval is greater than the start time of an already booked interval.

### Algorithm
1. Create a list to store the bookings as tuples.
2. On each booking request:
    - Check the new interval against each booked interval to detect overlaps.
    - If there is no overlap, add the interval to the list.

### Time Complexity
- Average Time Complexity: O(n^2) for `n` number of bookings, as we iterate over all previous bookings for each booking request.
- Space Complexity: O(n) to store booking intervals.

### Implementation
```csharp
public class MyCalendar {
    private List<(int start, int end)> bookings;

    public MyCalendar() {
        bookings = new List<(int, int)>();
    }
    
    public bool Book(int start, int end) {
        // Check for overlapping with existing bookings
        foreach (var (bookedStart, bookedEnd) in bookings) {
            if (start < bookedEnd && end > bookedStart) {
                // Overlapping intervals detected
                return false;
            }
        }
        // No overlapping, add the new booking
        bookings.Add((start, end));
        return true;
    }
}
```

## Approach 2: Binary Search Tree for Efficient Lookup

### Intuition
To improve the efficiency of checking for overlaps, we can use a Binary Search Tree (BST) to store the bookings. With BST, interval checks become faster because they can take advantage of ordering properties, reducing the number of comparisons.

### Algorithm
1. Use a balanced binary search tree (e.g., SortedList in C#) to keep track of booking intervals.
2. On each booking request:
    - Use binary search to efficiently find where the new interval fits in the current ordered list.
    - Check only adjacent intervals or the exact spot to determine if it overlaps.
    - Insert the interval if no overlaps are detected.

### Time Complexity
- Average Time Complexity: O(n log n) due to sorting operations and tree balancing.
- Space Complexity: O(n) to store booking intervals.

### Implementation
```csharp
public class MyCalendar {
    private SortedList<int, int> bookings;

    public MyCalendar() {
        bookings = new SortedList<int, int>();
    }
    
    public bool Book(int start, int end) {
        if (bookings.Count > 0) {
            var nextIndex = bookings.Keys.BinarySearch(start);
            if (nextIndex >= 0) {
                // start exists or is larger than all elements
                if (nextIndex < bookings.Count && bookings.Keys[nextIndex] < end) {
                    // Overlap detected with next interval
                    return false;
                }
            }
            else {
                // Correct the index to positive for insertion logic
                nextIndex = ~nextIndex;
                if (nextIndex > 0 && bookings.Values[nextIndex - 1] > start) {
                    // Overlap detected with previous interval
                    return false;
                }
            }
        }
        // No overlap found, add the booking
        bookings.Add(start, end);
        return true;
    }
}
```

This improved method leverages the properties of binary search tree operations which make the booking insertion checks more efficient as the number of booking requests grows.

