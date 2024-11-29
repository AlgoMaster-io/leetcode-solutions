# 260. [Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approach 1: HashMap for Counting Frequencies

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_map>
#include <vector>

class Solution {
public:
    std::vector<int> singleNumber(std::vector<int>& nums) {
        std::unordered_map<int, int> countMap;
        
        // Count the frequency of each number
        for (int num : nums) {
            countMap[num]++;
        }
        
        std::vector<int> result(2);
        int idx = 0;
        // Find the numbers with frequency 1
        for (const auto& entry : countMap) {
            if (entry.second == 1) {
                result[idx++] = entry.first;
            }
        }
        
        return result;
    }
};
```

## Approach 2: Bit Manipulation

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
#include <vector>

class Solution {
public:
    std::vector<int> singleNumber(std::vector<int>& nums) {
        int xorResult = 0;
        
        // XOR all numbers to get xor of the two unique numbers
        for (int num : nums) {
            xorResult ^= num;
        }
        
        // Find a set bit (rightmost) in xor to separate the numbers
        int diff = xorResult & (-xorResult);
        
        std::vector<int> result(2, 0);
        // Separate the numbers into two groups and XOR them respectively
        for (int num : nums) {
            if ((num & diff) == 0) {
                result[0] ^= num; // XOR for first group
            } else {
                result[1] ^= num; // XOR for second group
            }
        }
        
        return result;
    }
};
```

