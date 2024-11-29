# [1512. Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Approach 1: Brute Force

### Solution
c++
// Time Complexity: O(n^2)
// Space Complexity: O(1)
```cpp
class Solution {
public:
    int numIdenticalPairs(vector<int>& nums) {
        int count = 0;

        // Compare every pair of elements
        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) {
                if (nums[i] == nums[j]) {
                    count++; // Increment count if pair is "good"
                }
            }
        }

        return count;
    }
};
```

## Approach 2: Unordered Map for Counting Frequencies

### Solution
c++
// Time Complexity: O(n)
// Space Complexity: O(n)
```cpp
#include <unordered_map>
#include <vector>

using namespace std;

class Solution {
public:
    int numIdenticalPairs(vector<int>& nums) {
        unordered_map<int, int> countMap;
        int count = 0;

        // Count the occurrences of each number
        for (int num : nums) {
            if (countMap.find(num) != countMap.end()) {
                count += countMap[num]; // Add the current count of this number
            }
            countMap[num]++; // Increment the count
        }

        return count;
    }
};
```

## Approach 3: Frequency Array (Optimized for Small Range)

### Solution
c++
// Time Complexity: O(n)
// Space Complexity: O(1)
```cpp
#include <vector>

using namespace std;

class Solution {
public:
    int numIdenticalPairs(vector<int>& nums) {
        int freq[101] = {0}; // Frequency array for numbers in range [1, 100]
        int count = 0;

        // Count pairs using frequency
        for (int num : nums) {
            count += freq[num]; // Add the number of pairs that can be formed
            freq[num]++; // Increment the frequency of the current number
        }

        return count;
    }
};
```

