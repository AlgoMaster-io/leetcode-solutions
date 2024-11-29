# 300. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## Approach 1: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <vector>
#include <algorithm>

class Solution {
public:
    int lengthOfLIS(std::vector<int>& nums) {
        if (nums.empty()) {
            return 0;
        }

        std::vector<int> dp(nums.size(), 1); // dp array to store the LIS ending at each index
        int maxLength = 1; // Minimum LIS length is 1 (a single element)

        // Build the dp array
        for (int i = 1; i < nums.size(); i++) { 
            for (int j = 0; j < i; j++) { 
                if (nums[i] > nums[j]) { // Check if nums[i] can extend the subsequence at nums[j]
                    dp[i] = std::max(dp[i], dp[j] + 1); // Update dp[i] with the maximum length found
                }
            }
            maxLength = std::max(maxLength, dp[i]); // Update the overall LIS length
        }

        return maxLength;
    }
};
```

## Approach 2: Binary Search with Patience Sorting

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <vector>
#include <algorithm>

class Solution {
public:
    int lengthOfLIS(std::vector<int>& nums) {
        if (nums.empty()) {
            return 0;
        }

        std::vector<int> tails(nums.size(), 0); // Array to store the smallest tail for all increasing subsequences
        int size = 0; // Size of the effective array

        for (int num : nums) {
            int i = 0, j = size; // Binary search for the insertion point of num in tails

            while (i < j) {
                int mid = (i + j) / 2;
                if (tails[mid] < num) {
                    i = mid + 1;
                } else {
                    j = mid;
                }
            }

            tails[i] = num; // Replace or extend the increasing subsequence
            if (i == size) {
                size++; // Increment size if a new subsequence tail is created
            }
        }

        return size;
    }
};
```

