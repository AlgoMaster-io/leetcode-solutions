# [974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        int count = 0;

        // Check all subarrays
        for (int i = 0; i < nums.size(); i++) {
            int sum = 0;

            for (int j = i; j < nums.size(); j++) {
                sum += nums[j];

                // Check if the sum is divisible by k
                if (sum % k == 0) {
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
// Space Complexity: O(k)
#include <unordered_map>
#include <vector>
using namespace std;

class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        unordered_map<int, int> remainderMap;
        remainderMap[0] = 1; // Initialize with a remainder of 0
        
        int count = 0;
        int sum = 0;

        for (int num : nums) {
            sum += num;
            int remainder = sum % k;

            // Normalize the remainder to always be non-negative
            if (remainder < 0) {
                remainder += k;
            }

            // If the remainder exists in the map, add its frequency to the count
            if (remainderMap.count(remainder)) {
                count += remainderMap[remainder];
            }

            // Update the frequency of the current remainder
            remainderMap[remainder]++;
        }

        return count;
    }
};
```

