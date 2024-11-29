# [525. Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        int maxLength = 0;

        // Check all possible subarrays
        for (int i = 0; i < nums.size(); i++) {
            int count = 0;

            for (int j = i; j < nums.size(); j++) {
                // Increment or decrement count based on value
                count += nums[j] == 1 ? 1 : -1;

                // If count is zero, it means equal number of 0s and 1s
                if (count == 0) {
                    maxLength = max(maxLength, j - i + 1);
                }
            }
        }

        return maxLength;
    }
};
```

## Approach 2: HashMap (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_map>
#include <vector>
using namespace std;

class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        unordered_map<int, int> map;
        map[0] = -1; // Initialize with a count of 0 at index -1
        int maxLength = 0;
        int count = 0;

        for (int i = 0; i < nums.size(); i++) {
            // Increment or decrement count based on value
            count += nums[i] == 1 ? 1 : -1;

            if (map.find(count) != map.end()) {
                // Calculate the length of the subarray
                maxLength = max(maxLength, i - map[count]);
            } else {
                // Store the first occurrence of this count
                map[count] = i;
            }
        }

        return maxLength;
    }
};
```

