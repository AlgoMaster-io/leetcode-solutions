# [1499. Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approach 1: Using a Priority Queue (Heap-Based)

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <queue>
#include <vector>
#include <limits.h>

class Solution {
public:
    int findMaxValueOfEquation(std::vector<std::vector<int>>& points, int k) {
        std::priority_queue<std::pair<int, int>> maxHeap; // Max-heap: {y - x, x}
        int maxValue = INT_MIN;

        for (const auto& point : points) {
            int x = point[0], y = point[1];

            // Remove points outside the range of k
            while (!maxHeap.empty() && x - maxHeap.top().second > k) {
                maxHeap.pop();
            }

            // If the heap is not empty, calculate the current value of the equation
            if (!maxHeap.empty()) {
                maxValue = std::max(maxValue, maxHeap.top().first + y + x);
            }

            // Add the current point to the heap
            maxHeap.emplace(y - x, x);
        }

        return maxValue;
    }
};
```

## Approach 2: Using a Deque (Optimal Solution)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <deque>
#include <vector>
#include <limits.h>

class Solution {
public:
    int findMaxValueOfEquation(std::vector<std::vector<int>>& points, int k) {
        std::deque<std::pair<int, int>> deque; // Stores pairs {y - x, x}
        int maxValue = INT_MIN;

        for (const auto& point : points) {
            int x = point[0], y = point[1];

            // Remove points outside the range of k
            while (!deque.empty() && x - deque.front().second > k) {
                deque.pop_front();
            }

            // If the deque is not empty, calculate the current value of the equation
            if (!deque.empty()) {
                maxValue = std::max(maxValue, deque.front().first + y + x);
            }

            // Remove points from the back of the deque that have a smaller y - x value
            while (!deque.empty() && deque.back().first <= y - x) {
                deque.pop_back();
            }

            // Add the current point to the deque
            deque.emplace_back(y - x, x);
        }

        return maxValue;
    }
};
```

