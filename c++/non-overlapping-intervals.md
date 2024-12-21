# [Leetcode 435: Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

## Approaches
- [Greedy Approach](#greedy-approach)

### Greedy Approach

**Intuition:**

The problem of finding the minimum number of intervals to remove such that the rest of the intervals do not overlap can be tackled using a greedy approach. The key idea is to always keep the interval that ends the earliest. By doing so, we are leaving more space for the upcoming intervals which increases the chances of them fitting into the non-overlapping list.

**Approach:**

1. **Sort the Intervals**: Start by sorting the intervals based on their ending times. This sorting ensures that we always have a choice of picking the interval that ends the earliest.

2. **Iterate Through the Sorted Intervals**: As we go through the sorted intervals, we maintain a variable `prev_end` which keeps track of the end of the last added interval to our non-overlapping list.

3. **Selectively Add Intervals**: For each interval, compare its start with `prev_end`. If the current interval's start is greater than or equal to `prev_end`, it implies this interval can be added to our non-overlapping list.

4. **Count Overlaps**: If an interval overlaps with the last added interval (i.e., its start is less than `prev_end`), increment the count for intervals to be removed.

5. **Final Count**: The result is the accumulated count of intervals that need to be removed to achieve non-overlapping intervals throughout.

**Time Complexity:**

- **Sorting Step**: \(O(n \log n)\), where \(n\) is the number of intervals.
- **Iteration Step**: \(O(n)\) while checking for overlaps and counting them.

Overall, the time complexity is \(O(n \log n)\) due to the sorting step.

**Space Complexity:**

- \(O(1)\), if we exclude the space required for sorting which is typically \(O(n)\).

Here's the detailed implementation:

```cpp
#include <vector>
#include <algorithm>

class Solution {
public:
    int eraseOverlapIntervals(std::vector<std::vector<int>>& intervals) {
        // Sort intervals based on their end times
        std::sort(intervals.begin(), intervals.end(), [](const std::vector<int>& a, const std::vector<int>& b) {
            return a[1] < b[1];
        });
        
        int prev_end = intervals[0][1]; // Initialize with the end of the first interval
        int count = 0; // Count of intervals to remove

        // Iterate from the second interval
        for (int i = 1; i < intervals.size(); ++i) {
            if (intervals[i][0] < prev_end) {
                // Increment the removal count if there is an overlap
                ++count;
            } else {
                // Update the end if there is no overlap
                prev_end = intervals[i][1];
            }
        }

        return count;
    }
};
```

This solution efficiently determines the minimum number of intervals that need to be removed to prevent any overlap. By leveraging sorting and a greedy choice of selecting the next interval to minimize overlap, we attain an optimal solution.

