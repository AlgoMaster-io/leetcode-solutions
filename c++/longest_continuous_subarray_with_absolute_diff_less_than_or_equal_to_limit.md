# [1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Approach 1: Sliding Window with Deque

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <deque>
#include <vector>
#include <algorithm>

class Solution {
public:
    int longestSubarray(std::vector<int>& nums, int limit) {
        std::deque<int> maxDeque; // Stores indices of max elements in the window
        std::deque<int> minDeque; // Stores indices of min elements in the window
        int left = 0, maxLength = 0;

        for (int right = 0; right < nums.size(); ++right) {
            // Maintain the decreasing order in maxDeque
            while (!maxDeque.empty() && nums[maxDeque.back()] < nums[right]) {
                maxDeque.pop_back();
            }
            maxDeque.push_back(right);

            // Maintain the increasing order in minDeque
            while (!minDeque.empty() && nums[minDeque.back()] > nums[right]) {
                minDeque.pop_back();
            }
            minDeque.push_back(right);

            // Check if the current window is valid
            while (nums[maxDeque.front()] - nums[minDeque.front()] > limit) {
                left++; // Shrink the window from the left
                // Remove indices out of the window
                if (maxDeque.front() < left) maxDeque.pop_front();
                if (minDeque.front() < left) minDeque.pop_front();
            }

            maxLength = std::max(maxLength, right - left + 1); // Update the max length
        }

        return maxLength;
    }
};
```

## Approach 2: Sliding Window with TreeMap

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <map>
#include <vector>
#include <algorithm>

class Solution {
public:
    int longestSubarray(std::vector<int>& nums, int limit) {
        std::map<int, int> map; // Stores elements and their frequencies
        int left = 0, maxLength = 0;

        for (int right = 0; right < nums.size(); ++right) {
            // Add the current element to the map
            map[nums[right]]++;

            // Shrink the window if the condition is violated
            while (map.rbegin()->first - map.begin()->first > limit) {
                map[nums[left]]--;
                if (map[nums[left]] == 0) {
                    map.erase(nums[left]);
                }
                left++;
            }

            maxLength = std::max(maxLength, right - left + 1); // Update the max length
        }

        return maxLength;
    }
};
```

