# 673. [Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Approach 1: Dynamic Programming with Two Arrays

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <vector>
#include <algorithm>

class Solution {
public:
    int findNumberOfLIS(std::vector<int>& nums) {
        if (nums.empty()) return 0;

        int n = nums.size();
        std::vector<int> lengths(n, 1); // lengths[i] will be the length of the longest ending in nums[i]
        std::vector<int> counts(n, 1);  // counts[i] will be the number of the longest ending in nums[i]

        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[i] > nums[j]) {
                    if (lengths[j] + 1 > lengths[i]) {
                        lengths[i] = lengths[j] + 1;
                        counts[i] = counts[j];
                    } else if (lengths[j] + 1 == lengths[i]) {
                        counts[i] += counts[j];
                    }
                }
            }
        }

        int maxLength = 0, numberOfLIS = 0;
        for (int length : lengths) {
            maxLength = std::max(maxLength, length);
        }
        for (int i = 0; i < n; ++i) {
            if (lengths[i] == maxLength) {
                numberOfLIS += counts[i];
            }
        }

        return numberOfLIS;
    }
};
```

## Approach 2: Optimized Dynamic Programming with Fenwick Tree

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <vector>
#include <set>
#include <map>
#include <algorithm>

class Solution {
public:
    class FenwickTree {
    public:
        FenwickTree(int size) : lengths(size), counts(size) {}

        std::pair<int, int> query(int index) {
            int maxLength = 0, totalCounts = 0;
            for (; index > 0; index -= index & -index) {
                if (lengths[index] > maxLength) {
                    maxLength = lengths[index];
                    totalCounts = counts[index];
                } else if (lengths[index] == maxLength) {
                    totalCounts += counts[index];
                }
            }
            return {maxLength, totalCounts};
        }

        void update(int index, int length, int count) {
            for (; index < lengths.size(); index += index & -index) {
                if (lengths[index] < length) {
                    lengths[index] = length;
                    counts[index] = count;
                } else if (lengths[index] == length) {
                    counts[index] += count;
                }
            }
        }

    private:
        std::vector<int> lengths;
        std::vector<int> counts;
    };

    int findNumberOfLIS(std::vector<int>& nums) {
        if (nums.empty()) return 0;

        int offset = 1;
        std::set<int> unique_nums(nums.begin(), nums.end());
        int rank = 0;
        std::map<int, int> numToIndex;
        for (int num : unique_nums) {
            numToIndex[num] = ++rank;
        }

        FenwickTree fenwickTree(rank + offset);
        int maxLength = 0, result = 0;

        for (int num : nums) {
            int currentIndex = numToIndex[num];
            auto previous = fenwickTree.query(currentIndex - 1);
            int currentLength = previous.first + 1;
            int currentCount = std::max(previous.second, 1);

            fenwickTree.update(currentIndex, currentLength, currentCount);

            if (currentLength > maxLength) {
                maxLength = currentLength;
                result = currentCount;
            } else if (currentLength == maxLength) {
                result += currentCount;
            }
        }

        return result;
    }
};
```

