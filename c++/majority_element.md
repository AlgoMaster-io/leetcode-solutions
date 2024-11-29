# [169. Majority Element](https://leetcode.com/problems/majority-element/)

## Approach 1: Brute Force

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int majorityCount = nums.size() / 2;

        // Check the count of each element
        for (int num : nums) {
            int count = 0;
            for (int element : nums) {
                if (element == num) {
                    count++;
                }
            }
            // If count exceeds majority, return the element
            if (count > majorityCount) {
                return num;
            }
        }

        return -1; // Shouldn't reach here as per problem constraints
    }
};
```

## Approach 2: HashMap to Count Frequencies

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_map>
using namespace std;

class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> countMap;
        int majorityCount = nums.size() / 2;

        // Count frequencies of elements
        for (int num : nums) {
            countMap[num]++;

            // If an element's count exceeds majority, return it
            if (countMap[num] > majorityCount) {
                return num;
            }
        }

        return -1; // Shouldn't reach here as per problem constraints
    }
};
```

## Approach 3: Sorting

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(1)
#include <algorithm>
using namespace std;

class Solution {
public:
    int majorityElement(vector<int>& nums) {
        // Sort the array
        sort(nums.begin(), nums.end());

        // The majority element will always be at the middle index
        return nums[nums.size() / 2];
    }
};
```

## Approach 4: Boyer-Moore Voting Algorithm

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
#include <vector>
using namespace std;

class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int count = 0;
        int candidate = 0;

        // Find the candidate for the majority element
        for (int num : nums) {
            if (count == 0) {
                candidate = num; // Set a new candidate
            }
            count += (num == candidate) ? 1 : -1;
        }

        return candidate; // The problem guarantees the majority element exists
    }
};
```

