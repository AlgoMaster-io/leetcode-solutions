# [15. 3Sum](https://leetcode.com/problems/3sum/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n^3)
// Space Complexity: O(1) (excluding output list)
#include <vector>
#include <set>
#include <algorithm>

class Solution {
public:
    std::vector<std::vector<int>> threeSum(std::vector<int>& nums) {
        std::set<std::vector<int>> resultSet;
        int n = nums.size();

        // Iterate through all possible triplets
        for (int i = 0; i < n - 2; i++) {
            for (int j = i + 1; j < n - 1; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        std::vector<int> triplet = {nums[i], nums[j], nums[k]};
                        std::sort(triplet.begin(), triplet.end()); // Ensure the triplet is in sorted order
                        resultSet.insert(triplet); // Add triplet to the set to avoid duplicates
                    }
                }
            }
        }

        return std::vector<std::vector<int>>(resultSet.begin(), resultSet.end()); // Convert set to list for the output
    }
};
```

## Approach 2: Two Pointers (Optimal)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n) (for sorting or output list)
#include <vector>
#include <algorithm>

class Solution {
public:
    std::vector<std::vector<int>> threeSum(std::vector<int>& nums) {
        std::vector<std::vector<int>> result;
        std::sort(nums.begin(), nums.end()); // Sort the array to use two-pointer technique

        for (int i = 0; i < nums.size() - 2; i++) {
            // Skip duplicates for the first element
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            int left = i + 1;
            int right = nums.size() - 1;

            // Two-pointer approach
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum == 0) {
                    result.push_back({nums[i], nums[left], nums[right]});

                    // Skip duplicates for the second and third elements
                    while (left < right && nums[left] == nums[left + 1]) left++;
                    while (left < right && nums[right] == nums[right - 1]) right--;

                    left++;
                    right--;
                } else if (sum < 0) {
                    left++; // Increase the sum
                } else {
                    right--; // Decrease the sum
                }
            }
        }

        return result;
    }
};
```

