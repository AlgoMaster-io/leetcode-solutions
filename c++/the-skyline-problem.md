# [The Skyline Problem - Leetcode 218](https://leetcode.com/problems/the-skyline-problem/)

### Approaches
- [Brute Force Approach](#brute-force-approach)
- [Sweep Line Algorithm with Priority Queue](#sweep-line-algorithm-with-priority-queue)

## Brute Force Approach

### Intuition
In the brute force approach, we create an array that represents the height at each coordinate based on the given list of buildings. Then, we iterate through this height array to determine the skyline points. This approach helps us understand the basic concept of tracing over the overlap of buildings by directly creating a height map.

### Steps
1. Determine the maximum right end of the given buildings to manage the size of our height array.
2. For each building defined by three integers [l, r, h], iterate over each `x` from `l` to `r-1` and set the corresponding height in a `heights` array to the maximum height `h` seen at that point.
3. After finishing the height array, construct the result skyline by iterating through the heights and noting down the transition points where the height changes.

### Time Complexity
- **Time**: O((n + L) * m), where `n` is the number of buildings, `L` is the maximum coordinate range, and `m` is the bounded maximum right.
- **Space**: O(L), where `L` is the maximum right coordinate of any building.

### C++ Implementation
```cpp
#include <vector>
#include <algorithm>

std::vector<std::vector<int>> getSkyline(std::vector<std::vector<int>>& buildings) {
    int maxRight = 0;
    for (const auto& building : buildings) {
        maxRight = std::max(maxRight, building[1]);
    }

    std::vector<int> heights(maxRight + 1, 0);

    // Fill the height map.
    for (const auto& building : buildings) {
        int l = building[0], r = building[1], h = building[2];
        for (int i = l; i < r; ++i) {
            heights[i] = std::max(heights[i], h);
        }
    }

    std::vector<std::vector<int>> result;
    int currentHeight = 0;
    for (int i = 0; i <= maxRight; ++i) {
        if (heights[i] != currentHeight) {
            result.push_back({i, heights[i]});
            currentHeight = heights[i];
        }
    }
    return result;
}
```

## Sweep Line Algorithm with Priority Queue

### Intuition
The sweep line technique efficiently processes each building's start and end points using a priority queue (max heap). This method exploits the observation that each significant change in the skyline corresponds to either the start or the end of a building. By iterating over combined start and end events sorted by x-coordinate, we can dynamically track the current highest building at each point.

### Steps
1. For each building, translate it into two events: entering at x=l with height h, and leaving at x=r+offset with height 0.
2. Sort these events. Process them by iterating through the sorted list:
   - Add entering events to the max-heap.
   - Remove leaving events from the max-heap.
   - At each x-coordinate, check the current maximum height of the active heap. If it's different from the last recorded height, it means there's a skyline change.
3. Every time there's a change in height, add a new point to the skyline.

### Time Complexity
- **Time**: O(n log n), where `n` is the number of buildings due to sorting and heap operations.
- **Space**: O(n), for storing events and the priority queue.

### C++ Implementation
```cpp
#include <vector>
#include <set>
#include <algorithm>
#include <queue>

std::vector<std::vector<int>> getSkyline(std::vector<std::vector<int>>& buildings) {
    std::vector<std::pair<int, int>> events;
    for (auto& building : buildings) {
        events.emplace_back(building[0], -building[2]); // Starting point with negative height
        events.emplace_back(building[1], building[2]); // Ending point with positive height
    }

    // Sort events, primary on x, secondary on height (neglecting to prioritize entering points)
    std::sort(events.begin(), events.end());

    std::vector<std::vector<int>> result;
    std::multiset<int> activeHeights = {0};  // Multiset to keep heights sorted automatically
    int prevHeight = 0;

    for (auto& event : events) {
        int x = event.first;
        int h = event.second;

        if (h < 0) {  // Starting point, add the height to the active heights
            activeHeights.insert(-h);
        } else {  // Removing height as the building ends
            activeHeights.erase(activeHeights.find(h));
        }

        // Current maximum height
        int currentHeight = *activeHeights.rbegin();
        if (currentHeight != prevHeight) {
            result.push_back({x, currentHeight});
            prevHeight = currentHeight;
        }
    }
    return result;
}
```

In summary, the brute force method is useful for understanding the problem, while the Sweep Line Algorithm with a priority queue is more efficient, especially for larger datasets.

