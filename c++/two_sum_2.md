# [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## Approach 1: Binary Search for the Complement

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(1)
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for (int i = 0; i < numbers.size(); i++) {
            int complement = target - numbers[i];
            int index = binarySearch(numbers, i + 1, numbers.size() - 1, complement);

            if (index != -1) {
                return {i + 1, index + 1}; // 1-based index
            }
        }
        return {-1, -1}; // No solution found
    }

private:
    int binarySearch(vector<int>& numbers, int left, int right, int target) {
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (numbers[mid] == target) {
                return mid;
            } else if (numbers[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1; // Target not found
    }
};
```

## Approach 2: HashMap (For Non-Sorted Input)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <vector>
#include <unordered_map>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        unordered_map<int, int> map;

        for (int i = 0; i < numbers.size(); i++) {
            int complement = target - numbers[i];

            if (map.find(complement) != map.end()) {
                return {map[complement] + 1, i + 1}; // 1-based index
            }

            map[numbers[i]] = i;
        }

        return {-1, -1}; // No solution found
    }
};
```

## Approach 3: Two Pointers

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int left = 0; // Start pointer
        int right = numbers.size() - 1; // End pointer

        while (left < right) {
            int sum = numbers[left] + numbers[right];
            
            if (sum == target) {
                return {left + 1, right + 1}; // 1-based index
            } else if (sum < target) {
                left++; // Move left pointer to increase sum
            } else {
                right--; // Move right pointer to decrease sum
            }
        }

        return {-1, -1}; // No solution found
    }
};
```

