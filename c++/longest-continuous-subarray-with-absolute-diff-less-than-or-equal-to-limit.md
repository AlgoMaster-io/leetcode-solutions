# [Leetcode 1438: Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Sliding Window & Deque Approach](#sliding-window--deque-approach)

### Brute Force Approach

#### Intuition:
The brute force approach involves examining every possible subarray in the array to check whether the absolute difference condition is satisfied. While simple to implement, this approach is inefficient for larger arrays.

The main idea is to iterate over each starting point of the subarrays, and for each start point, iterate through every possible end point to determine if the subarray satisfies the absolute difference condition with respect to the given limit.

#### Time Complexity:
- **Time Complexity:** O(n^3), where n is the size of the array. This is due to the nested loops that consider all possible subarrays and check conditions within another loop.
- **Space Complexity:** O(1), because no extra space proportional to input size is used.

#### C++ Code:
```cpp
#include <vector>
#include <limits>
#include <cmath>

int longestSubarrayBruteForce(std::vector<int>& nums, int limit) {
    int maxLength = 0;
    int n = nums.size();
    
    // Examine all subarrays
    for (int start = 0; start < n; ++start) {
        for (int end = start; end < n; ++end) {
            int minElem = std::numeric_limits<int>::max();
            int maxElem = std::numeric_limits<int>::min();
            // Find min and max in the current subarray
            for (int k = start; k <= end; ++k) {
                minElem = std::min(minElem, nums[k]);
                maxElem = std::max(maxElem, nums[k]);
            }
            // Check if the subarray satisfies the limit condition
            if (std::abs(maxElem - minElem) <= limit) {
                maxLength = std::max(maxLength, end - start + 1);
            }
        }
    }
    
    return maxLength;
}
```

### Sliding Window & Deque Approach

#### Intuition:
An optimal approach involves utilizing two deques to keep track of the minimum and maximum elements in the current subarray in a sliding window fashion. This reduces the need to repeatedly traverse and find the min/max elements in the subarray.

We maintain a window defined by two pointers (start and end). The `maxDeque` is used to store potential max elements of the subarray in a way that its front always gives the maximum value, and `minDeque` likewise gives the minimum value. As we expand the window, we adjust the start pointer whenever the absolute difference exceeds `limit`.

#### Time Complexity:
- **Time Complexity:** O(n), where n is the number of elements in the array. Each element is inserted and removed from deques at most once.
- **Space Complexity:** O(n), due to the use of deques which could potentially store all elements of the array.

#### C++ Code:
```cpp
#include <vector>
#include <deque>

int longestSubarray(std::vector<int>& nums, int limit) {
    std::deque<int> maxDeque, minDeque;
    int start = 0, maxLength = 0;
    
    for (int end = 0; end < nums.size(); ++end) {
        // Maintain the maxDeque to store indices of potential max values
        while (!maxDeque.empty() && nums[maxDeque.back()] <= nums[end]) {
            maxDeque.pop_back();
        }
        maxDeque.push_back(end);
        
        // Maintain the minDeque to store indices of potential min values
        while (!minDeque.empty() && nums[minDeque.back()] >= nums[end]) {
            minDeque.pop_back();
        }
        minDeque.push_back(end);
        
        // Adjust the window if the absolute difference condition is violated
        while (nums[maxDeque.front()] - nums[minDeque.front()] > limit) {
            start++;
            if (maxDeque.front() < start) maxDeque.pop_front();
            if (minDeque.front() < start) minDeque.pop_front();
        }
        
        // Calculate the maximum length of the valid subarray
        maxLength = std::max(maxLength, end - start + 1);
    }
    
    return maxLength;
}
```

In this optimal approach, the sliding window and deques ensure we efficiently manage the current subarray's max and min values, allowing us to solve the problem in linear time.

