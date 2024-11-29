# 729. [My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approach 1: Brute Force with List of Intervals

### Solution
c++
// Time Complexity: O(n^2), where n is the number of events booked
// Space Complexity: O(n), where n is the number of events stored
#include <vector>
using namespace std;

class MyCalendar {
private:
    vector<pair<int, int>> calendar;

public:
    MyCalendar() {
    }

    bool book(int start, int end) {
        for (auto event : calendar) {
            // Check if the new event overlaps with any existing event
            if (start < event.second && end > event.first) {
                return false; // Overlap found, booking fails
            }
        }
        calendar.push_back({start, end}); // No overlap, booking succeeds
        return true;
    }
};


## Approach 2: TreeMap for Efficient Overlap Check

### Solution
c++
// Time Complexity: O(log n) per booking, where n is the number of events
// Space Complexity: O(n), where n is the number of events stored
#include <map>
using namespace std;

class MyCalendar {
private:
    map<int, int> calendar;

public:
    MyCalendar() {
    }

    bool book(int start, int end) {
        // Get the closest event that starts before the new event
        auto it = calendar.upper_bound(start);
        // Check for overlap with the previous or next event
        if (it != calendar.begin() && prev(it)->second > start) {
            return false; // Overlap found, booking fails
        }
        if (it != calendar.end() && it->first < end) {
            return false; // Overlap found, booking fails
        }
        calendar[start] = end; // No overlap, booking succeeds
        return true;
    }
};

