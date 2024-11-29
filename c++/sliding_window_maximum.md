# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Approach 1: Using Deque (Optimal Solution)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(k)
#include <vector>
#include <deque>

class Solution {
public:
    std::vector<int> maxSlidingWindow(std::vector<int>& nums, int k) {
        int n = nums.size();
        std::vector<int> result(n - k + 1);
        std::deque<int> deque; // Stores indices of elements

        for (int i = 0; i < n; ++i) {
            // Remove indices out of the current window
            if (!deque.empty() && deque.front() < i - k + 1) {
                deque.pop_front();
            }

            // Remove elements smaller than the current element from the back
            while (!deque.empty() && nums[deque.back()] < nums[i]) {
                deque.pop_back();
            }

            deque.push_back(i); // Add the current element's index to the deque

            // Add the maximum element of the current window to the result
            if (i >= k - 1) {
                result[i - k + 1] = nums[deque.front()];
            }
        }

        return result;
    }
};
```

## Approach 2: Using Priority Queue (Heap-Based)

### Solution
```cpp
// Time Complexity: O(n log k)
// Space Complexity: O(k)
#include <vector>
#include <queue>

class Solution {
public:
    std::vector<int> maxSlidingWindow(std::vector<int>& nums, int k) {
        int n = nums.size();
        std::vector<int> result(n - k + 1);
        std::priority_queue<std::pair<int, int>> maxHeap; // Max-heap: {value, index}

        for (int i = 0; i < n; ++i) {
            // Add the current element to the heap
            maxHeap.emplace(nums[i], i);

            // Remove elements that are out of the current window
            while (maxHeap.top().second < i - k + 1) {
                maxHeap.pop();
            }

            // Add the maximum element of the current window to the result
            if (i >= k - 1) {
                result[i - k + 1] = maxHeap.top().first;
            }
        }

        return result;
    }
};
```

