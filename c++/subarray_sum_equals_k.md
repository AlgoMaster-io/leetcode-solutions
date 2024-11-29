# [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int count = 0;

        // Check all subarrays
        for (int i = 0; i < nums.size(); i++) {
            int sum = 0;

            for (int j = i; j < nums.size(); j++) {
                sum += nums[j];

                // If the sum equals k, increment the count
                if (sum == k) {
                    count++;
                }
            }
        }

        return count;
    }
};
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_map>
#include <vector>
using namespace std;

class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> prefixSumMap;
        prefixSumMap[0] = 1; // Initialize with a prefix sum of 0

        int count = 0;
        int sum = 0;

        for (int num : nums) {
            sum += num; // Update the prefix sum

            // Check if there's a prefix sum that makes the current sum equal to k
            if (prefixSumMap.find(sum - k) != prefixSumMap.end()) {
                count += prefixSumMap[sum - k];
            }

            // Update the frequency of the current prefix sum
            prefixSumMap[sum]++;
        }

        return count;
    }
};
```

