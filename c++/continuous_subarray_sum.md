# [523. Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
#include <vector>

class Solution {
public:
    bool checkSubarraySum(std::vector<int>& nums, int k) {
        int n = nums.size();

        // Check every possible subarray
        for (int i = 0; i < n - 1; i++) {
            int sum = nums[i];
            for (int j = i + 1; j < n; j++) {
                sum += nums[j];
                if (k == 0 ? sum == 0 : sum % k == 0) {
                    return true; // Subarray sum is a multiple of k
                }
            }
        }

        return false; // No valid subarray found
    }
};
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <vector>
#include <unordered_map>

class Solution {
public:
    bool checkSubarraySum(std::vector<int>& nums, int k) {
        std::unordered_map<int, int> remainderMap;
        remainderMap[0] = -1; // Initialize with a remainder of 0 at index -1
        int sum = 0;

        for (int i = 0; i < nums.size(); i++) {
            sum += nums[i];

            // Compute the remainder when divided by k
            int remainder = k == 0 ? sum : sum % k;

            // If the remainder already exists
            if (remainderMap.find(remainder) != remainderMap.end()) {
                // Check if the subarray length is at least 2
                if (i - remainderMap[remainder] > 1) {
                    return true;
                }
            } else {
                // Store the index of this remainder
                remainderMap[remainder] = i;
            }
        }

        return false; // No valid subarray found
    }
};
```

