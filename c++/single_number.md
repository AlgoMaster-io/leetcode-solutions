# 136. [Single Number](https://leetcode.com/problems/single-number/)

## Approach 1: Using HashMap

### Solution
c++
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_map>
#include <vector>

class Solution {
public:
    int singleNumber(std::vector<int>& nums) {
        std::unordered_map<int, int> countMap;

        // Count the frequency of each number
        for (int num : nums) {
            countMap[num]++;
        }

        // Find the number with frequency 1
        for (int num : nums) {
            if (countMap[num] == 1) {
                return num;
            }
        }

        return -1; // No unique number found
    }
};


## Approach 2: Using Sorting

### Solution
c++
// Time Complexity: O(n log n)
// Space Complexity: O(1)
#include <algorithm>
#include <vector>

class Solution {
public:
    int singleNumber(std::vector<int>& nums) {
        std::sort(nums.begin(), nums.end()); // Sort the array

        // Traverse the array looking for unique element
        for (int i = 0; i < nums.size() - 1; i += 2) {
            if (nums[i] != nums[i + 1]) {
                return nums[i];
            }
        }

        return nums[nums.size() - 1]; // The last element is the single number
    }
};


## Approach 3: Using XOR

### Solution
c++
// Time Complexity: O(n)
// Space Complexity: O(1)
#include <vector>

class Solution {
public:
    int singleNumber(std::vector<int>& nums) {
        int result = 0;

        // XOR all elements, duplicate elements will cancel out
        for (int num : nums) {
            result ^= num;
        }

        return result; // The remaining single number
    }
};

