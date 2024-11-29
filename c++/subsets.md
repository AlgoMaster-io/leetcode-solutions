# 78. [Subsets](https://leetcode.com/problems/subsets/)

## Approach 1: Iterative (Power Set)

### Solution
```cpp
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n * 2^n) for storing the subsets
#include <vector>

class Solution {
public:
    std::vector<std::vector<int>> subsets(std::vector<int>& nums) {
        std::vector<std::vector<int>> result;
        result.push_back({}); // Start with the empty subset

        for (int num : nums) {
            int size = result.size();
            for (int i = 0; i < size; i++) {
                std::vector<int> subset = result[i];
                subset.push_back(num); // Add the current number to each existing subset
                result.push_back(subset);
            }
        }

        return result;
    }
};
```

## Approach 2: Backtracking (Recursive Subset Generation)

### Solution
```cpp
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n) for the recursion stack
#include <vector>

class Solution {
public:
    std::vector<std::vector<int>> subsets(std::vector<int>& nums) {
        std::vector<std::vector<int>> result;
        std::vector<int> current;
        backtrack(nums, 0, current, result);
        return result;
    }

private:
    void backtrack(std::vector<int>& nums, int index, std::vector<int>& current, std::vector<std::vector<int>>& result) {
        result.push_back(current); // Add the current subset to the result

        for (int i = index; i < nums.size(); i++) {
            current.push_back(nums[i]); // Include nums[i] in the current subset
            backtrack(nums, i + 1, current, result); // Recurse with the next index
            current.pop_back(); // Backtrack to explore other subsets
        }
    }
};
```

## Approach 3: Bitmasking (Iterative Subset Generation)

### Solution
```cpp
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n * 2^n) for storing the subsets
#include <vector>

class Solution {
public:
    std::vector<std::vector<int>> subsets(std::vector<int>& nums) {
        std::vector<std::vector<int>> result;
        int totalSubsets = 1 << nums.size(); // Total subsets = 2^n

        for (int mask = 0; mask < totalSubsets; mask++) {
            std::vector<int> subset;
            for (int i = 0; i < nums.size(); i++) {
                if ((mask & (1 << i)) != 0) {
                    subset.push_back(nums[i]); // Include nums[i] in the subset
                }
            }
            result.push_back(subset);
        }

        return result;
    }
};
```

