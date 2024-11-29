# [164. Maximum Gap](https://leetcode.com/problems/maximum-gap/)

## Approach 1: Sorting

### Solution
cpp
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(1) (ignoring the space required for sorting)
#include <algorithm>
#include <vector>
#include <climits>

class Solution {
public:
    int maximumGap(std::vector<int>& nums) {
        if (nums.empty() || nums.size() < 2) {
            return 0;
        }

        // Sort the array
        std::sort(nums.begin(), nums.end());

        int maxGap = 0;

        // Calculate the maximum gap between consecutive elements
        for (int i = 1; i < nums.size(); i++) {
            maxGap = std::max(maxGap, nums[i] - nums[i - 1]);
        }

        return maxGap;
    }
};
```

## Approach 2: Bucket Sort (Using Pigeonhole Principle)

### Solution
cpp
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <vector>
#include <algorithm>
#include <cmath>
#include <climits>

class Solution {
public:
    int maximumGap(std::vector<int>& nums) {
        if (nums.empty() || nums.size() < 2) {
            return 0;
        }

        int minVal = INT_MAX, maxVal = INT_MIN;
        for (int num : nums) {
            minVal = std::min(minVal, num);
            maxVal = std::max(maxVal, num);
        }

        // Calculate the gap size and number of buckets
        int n = nums.size();
        int gap = std::ceil(static_cast<double>(maxVal - minVal) / (n - 1));
        std::vector<int> bucketMin(n - 1, INT_MAX);
        std::vector<int> bucketMax(n - 1, INT_MIN);

        // Place each number into a bucket
        for (int num : nums) {
            if (num == minVal || num == maxVal) {
                continue;
            }
            int index = (num - minVal) / gap;
            bucketMin[index] = std::min(bucketMin[index], num);
            bucketMax[index] = std::max(bucketMax[index], num);
        }

        // Calculate the maximum gap
        int maxGap = 0, previous = minVal;
        for (int i = 0; i < n - 1; i++) {
            if (bucketMin[i] == INT_MAX) {
                continue; // Skip empty buckets
            }
            maxGap = std::max(maxGap, bucketMin[i] - previous);
            previous = bucketMax[i];
        }
        maxGap = std::max(maxGap, maxVal - previous); // Final gap with the max element

        return maxGap;
    }
};
```

