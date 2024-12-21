### [Leetcode Problem 731: My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

### Approaches:

1. [Approach 1: Brute Force using Two Lists](#approach-1-brute-force-using-two-lists)
2. [Approach 2: Optimized Booking using Event Sorting and Sweeping Line](#approach-2-optimized-booking-using-event-sorting-and-sweeping-line)

---

## Approach 1: Brute Force using Two Lists

### Intuition:
In this approach, we maintain two separate lists: one for all successful bookings (with no overlaps required) and another for double bookings (where the booking has overlapped once already). For each incoming booking request, we check it against all existing double bookings. If there's any overlap, we cannot book and must reject the booking. Otherwise, we attempt to overlap the new booking with existing successful bookings, potentially creating new double bookings.

### Implementation:

```cpp
#include <vector>
using namespace std;

class MyCalendarTwo {
public:
    vector<pair<int, int>> bookings;
    vector<pair<int, int>> doubleBookings;
    
    MyCalendarTwo() {
        
    }
    
    bool book(int start, int end) {
        // Check for triple bookings:
        for (auto &dupe : doubleBookings) {
            // If there is an overlap
            if (max(dupe.first, start) < min(dupe.second, end)) {
                return false;
            }
        }
        
        // Check for possible double booking and store if valid:
        for (auto &book : bookings) {
            // If there is an overlap
            if (max(book.first, start) < min(book.second, end)) {
                doubleBookings.push_back({max(book.first, start), min(book.second, end)});
            }
        }
        
        // Book the new entry
        bookings.push_back({start, end});
        return true;
    }
};
```

### Complexity Analysis:
- **Time Complexity:** O(N^2), where N is the number of bookings. We potentially compare each new booking with all existing double bookings and successful bookings.
- **Space Complexity:** O(N), due to storage of all bookings and overlaps.

---

## Approach 2: Optimized Booking using Event Sorting and Sweeping Line

### Intuition:
Instead of explicitly storing bookings and overlaps, we'll leverage a sweeping line algorithm using an event-based approach. We consider each timepoint as an event — starting or ending of a booking — and track the changes in the number of active events. A valid booking is one that never causes more than two ongoing events simultaneously.

### Implementation:

```cpp
#include <map>
using namespace std;

class MyCalendarTwo {
public:
    map<int, int> events;
    
    bool book(int start, int end) {
        // Marking the start and the end of the booking event
        events[start]++;
        events[end]--;
        
        // Traverse through the events map to check if there are triple bookings
        int activeBookings = 0;
        for (const auto &[time, count] : events) {
            activeBookings += count;
            // If at any point we have more than two active bookings at the same time
            if (activeBookings > 2) {
                // Revert the current booking as it causes more than two overlaps
                events[start]--;
                events[end]++;
                return false;
            }
        }
        return true;
    }
};
```

### Complexity Analysis:
- **Time Complexity:** O(N log N), where N is the number of events, because of logarithmic map insertions and order traversal.
- **Space Complexity:** O(N), as we store events in the map.

This approach is significantly more efficient for handling larger booking inputs compared to the brute force approach, due to the reduction in the complexity of overlap-checking logic through efficient event counting.

---

