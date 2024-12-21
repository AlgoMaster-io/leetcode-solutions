# [Leetcode 1353: Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Greedy Approach with Sorting](#approach-2-greedy-approach-with-sorting)
- [Approach 3: Priority Queue](#approach-3-priority-queue)

### Approach 1: Brute Force
The simplest approach is to try attending each event by looping through each day and marking the events attended. This approach is straightforward but not efficient.

#### Intuition
- For each day, try to attend an event if possible.
- Mark an event as attended for a given day and move to the next day or event.

#### Time Complexity
- O(n * d), where n is the number of events and d is the range of days. This is often infeasible for large inputs.

#### Space Complexity
- O(1), since we are not using any additional structures besides a counter for attended events.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int maxEvents(vector<vector<int>>& events) {
    sort(events.begin(), events.end());
    int maxAttendable = 0;
    vector<bool> attended(100001, false);  // assuming a constraint on the number of days

    for (auto& event : events) {
        int start = event[0];
        int end = event[1];
        for (int d = start; d <= end; ++d) {
            if (!attended[d]) {
                attended[d] = true;  // mark the event attended on day d
                maxAttendable++;
                break;
            }
        }
    }
    return maxAttendable;
}

int main() {
    vector<vector<int>> events = {{1,2},{2,3},{3,4}};
    cout << "Maximum events attended: " << maxEvents(events) << endl;
    return 0;
}
```

### Approach 2: Greedy Approach with Sorting
Sort all events by their end date, and try attending events starting from the earliest available date. 

#### Intuition
- Attend the events with the earliest end date first; this method maximizes the number of attendable events by finishing events sooner.
- Iterate over events and select the earliest possible available day to attend each event.

#### Time Complexity
- O(n log n) due to sorting the events based on the end day.

#### Space Complexity
- O(n) due to the auxiliary data structures used to keep track of attended days.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int maxEvents(vector<vector<int>>& events) {
    sort(events.begin(), events.end(), [](const vector<int>& a, const vector<int>& b) {
        return a[1] < b[1];  // Sort by end time
    });

    int maxAttendable = 0;
    int lastDay = 0;

    vector<bool> isDayTaken(100001, false);

    for (auto& event : events) {
        for (int d = event[0]; d <= event[1]; ++d) {
            if (!isDayTaken[d]) {
                isDayTaken[d] = true;
                maxAttendable++;
                break;
            }
        }
    }
    return maxAttendable;
}

int main() {
    vector<vector<int>> events = {{1,2},{2,3},{3,4}};
    cout << "Maximum events attended: " << maxEvents(events) << endl;
    return 0;
}
```

### Approach 3: Priority Queue
Use a priority queue to efficiently manage and choose the next event to attend based on the earliest ending times.

#### Intuition
- Iterate through each day from the start to the maximum end day of any event.
- Use a priority queue (or min-heap) to keep track of ongoing events sorted by their ending day.
- Always attend the event that ends the earliest but is still feasible to attend on a given day.

#### Time Complexity
- O(n log n), where n is the number of events, primarily dominated by sorting of events and heap operations.

#### Space Complexity
- O(n) for storing events in the priority queue.

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

int maxEvents(vector<vector<int>>& events) {
    // Sort events based on the starting time
    sort(events.begin(), events.end());
    priority_queue<int, vector<int>, greater<int>> pq;

    int maxAttendable = 0;
    int day = events[0][0];
    size_t i = 0;

    while (!pq.empty() || i < events.size()) {
        // Add all events that have started by this day to the priority queue
        while (i < events.size() && events[i][0] <= day) {
            pq.push(events[i][1]);
            ++i;
        }
        
        // Remove all ended events before the current day
        while (!pq.empty() && pq.top() < day) {
            pq.pop();
        }

        // Attend the event that ends the soonest
        if (!pq.empty()) {
            pq.pop();
            maxAttendable++;
        }
        
        day++;
    }

    return maxAttendable;
}

int main() {
    vector<vector<int>> events = {{1,2},{2,3},{3,4}};
    cout << "Maximum events attended: " << maxEvents(events) << endl;
    return 0;
}
```

This code gives a detailed step-by-step understanding and comments for each stage, ensuring you grasp the logic and structure clearly.

