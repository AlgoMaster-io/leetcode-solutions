# 528. [Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approach 1: Linear Search for Picking (Alternative to Binary Search)

### Solution
```cpp
// Time Complexity: O(n) for initialization, O(n) for each pick
// Space Complexity: O(n)
#include <vector>
#include <random>

class Solution {
private:
    std::vector<int> prefixSums;
    std::default_random_engine generator;
    std::uniform_int_distribution<int> distribution;
    
public:
    Solution(std::vector<int>& w) {
        prefixSums.resize(w.size());
        prefixSums[0] = w[0];
        for (size_t i = 1; i < w.size(); ++i) {
            prefixSums[i] = prefixSums[i - 1] + w[i];
        }
        distribution = std::uniform_int_distribution<int>(1, prefixSums.back());
    }

    int pickIndex() {
        int target = distribution(generator);

        for (size_t i = 0; i < prefixSums.size(); ++i) {
            if (target <= prefixSums[i]) {
                return i;
            }
        }
        
        return -1;
    }
};
```

## Approach 2: Prefix Sum Array and Binary Search

### Solution
```cpp
// Time Complexity: O(n) for initialization, O(log n) for each pick
// Space Complexity: O(n)
#include <vector>
#include <random>

class Solution {
private:
    std::vector<int> prefixSums;
    std::default_random_engine generator;
    std::uniform_int_distribution<int> distribution;
    
public:
    Solution(std::vector<int>& w) {
        prefixSums.resize(w.size());
        prefixSums[0] = w[0];
        for (size_t i = 1; i < w.size(); ++i) {
            prefixSums[i] = prefixSums[i - 1] + w[i];
        }
        distribution = std::uniform_int_distribution<int>(1, prefixSums.back());
    }

    int pickIndex() {
        int target = distribution(generator);

        // Binary search to find the target index
        int start = 0, end = prefixSums.size() - 1;
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (prefixSums[mid] < target) {
                start = mid + 1;
            } else {
                end = mid;
            }
        }
        
        return start;
    }
};
```

