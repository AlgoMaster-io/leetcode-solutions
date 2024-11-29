# 731. [My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

## Approach: Line Sweeping Using Map

### Solution
```cpp
// Time Complexity: O(n^2), where n is the number of events booked
// Space Complexity: O(n), where n is the number of events stored in the map
#include <map>

class MyCalendarTwo {

private:
    std::map<int, int> events;

public:
    MyCalendarTwo() {
        // Initialize events map
    }

    bool book(int start, int end) {
        // Increment count at the start of the event
        events[start]++;
        // Decrement count at the end of the event
        events[end]--;

        int active = 0; // Tracks active bookings at any time
        for (const auto& event : events) {
            active += event.second;
            if (active >= 3) {
                // Triple booking found, revert changes and return false
                events[start]--;
                if (events[start] == 0) {
                    events.erase(start);
                }
                events[end]++;
                if (events[end] == 0) {
                    events.erase(end);
                }
                return false;
            }
        }

        return true; // No triple booking, booking succeeds
    }
};
```

