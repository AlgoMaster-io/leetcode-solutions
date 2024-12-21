# [Leetcode 729: My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approaches:
1. [Brute Force using List](#approach-1)
2. [Optimized Approach using TreeMap (Balanced Binary Search Tree)](#approach-2)

---

## Approach 1: Brute Force using List

### Intuition:
The simplest way to solve the My Calendar I problem is to keep a list of events and for each new event request, we check if it conflicts with any of the existing events in the list. If there's no conflict, we add the new event to the list.

### Steps:
1. Use a list (or vector in C++) to store events, where each event is represented as a pair `(start, end)`.
2. For each `book` request, iterate over the list and check if the new event overlaps with any existing event.
3. The new event `(start, end)` overlaps with an existing event `(s, e)` if `max(s, start) < min(e, end)`.
4. If no overlap is found, append the new event to the list and return `true`.
5. If there's an overlap, return `false`.

### Code:
```cpp
class MyCalendar {
private:
    std::vector<std::pair<int, int>> events;

public:
    MyCalendar() {}

    bool book(int start, int end) {
        // Iterate over all existing events
        for (auto& event : events) {
            // Check if the new event overlaps with the current event
            if (std::max(event.first, start) < std::min(event.second, end)) {
                return false; // Overlap found
            }
        }
        // No overlap found, add the event
        events.push_back({start, end});
        return true;
    }
};
```

### Time Complexity:
- Each booking operation involves a linear scan of the events list, so the time complexity is \(O(N)\), where \(N\) is the number of events booked so far.

### Space Complexity:
- The space complexity is \(O(N)\), as we store at most \(N\) events.

---

## Approach 2: Optimized Approach using TreeMap (Balanced Binary Search Tree)

### Intuition:
To optimize the booking operation, we can use a data structure that maintains a sorted order of the events based on their start times. A map (or in C++ a `map<int, int>`) provides this capability and allows us to efficiently find and insert events.

### Steps:
1. Maintain a `map` where keys are start times and values are end times.
2. For each `book` request, find the earliest existing event that starts after or at the start of the new event.
3. Check if the new event ends before this found event starts.
4. Also check the event that ends just before or at the start of the new event (lower bound check).
5. Only add the new event if it doesn't overlap with any existing events found in steps 2 and 3.

### Code:
```cpp
class MyCalendar {
private:
    std::map<int, int> events;

public:
    MyCalendar() {}

    bool book(int start, int end) {
        // Find the first event that starts after 'start'
        auto next_event = events.lower_bound(start);

        // Check if it overlaps with the next event
        if (next_event != events.end() && next_event->first < end) {
            return false; // Overlaps with next event
        }

        // Check if it overlaps with the previous event
        if (next_event != events.begin()) {
            auto prev_event = std::prev(next_event);
            if (prev_event->second > start) {
                return false; // Overlaps with previous event
            }
        }

        // Add the event as no overlap found
        events[start] = end;
        return true;
    }
};
```

### Time Complexity:
- Each booking involves log-time operations for insertion and lookup due to the balanced tree nature of the `map`, resulting in \(O(\log N)\) time complexity for each booking.

### Space Complexity:
- The space complexity remains \(O(N)\) as we store at most \(N\) events. 

Both of these approaches solve the problem, but using a balanced binary search tree drastically improves booking times when the number of bookings \(N\) is large.

