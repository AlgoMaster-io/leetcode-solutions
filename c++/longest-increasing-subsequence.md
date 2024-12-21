# [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

### Approaches
- [Approach 1: Dynamic Programming](#approach-1-dynamic-programming)
- [Approach 2: Binary Search with Patience Sorting](#approach-2-binary-search-with-patience-sorting)

## Approach 1: Dynamic Programming

### Intuition
The idea is to use dynamic programming to calculate the longest increasing subsequence up to each index. We maintain a `dp` array where `dp[i]` represents the length of the longest increasing subsequence ending with `nums[i]`. We fill out this array by comparing each element with every previous element.

### Steps
1. Create a `dp` array initialized with 1s since the minimum subsequence ending at any element itself is 1.
2. For each element `nums[i]`, iterate through all previous elements `nums[j]` (where `j < i`) to check:
   - If `nums[i]` is greater than `nums[j]`, it can extend the subsequence ending at `nums[j]`.
   - Update `dp[i]` as `max(dp[i], dp[j] + 1)` if it forms a longer subsequence.
3. After processing all elements, the result is the maximum value in the `dp` array.

### Code

```cpp
#include <vector>
#include <algorithm>

class Solution {
public:
    int lengthOfLIS(std::vector<int>& nums) {
        if(nums.empty()) return 0;
        
        std::vector<int> dp(nums.size(), 1);
        
        for(size_t i = 1; i < nums.size(); ++i) {
            for(size_t j = 0; j < i; ++j) {
                if(nums[i] > nums[j]) {
                    dp[i] = std::max(dp[i], dp[j] + 1);
                }
            }
        }
        
        return *std::max_element(dp.begin(), dp.end()); // Get the maximum length
    }
};
```

### Complexity Analysis
- **Time Complexity**: \(O(n^2)\), where \(n\) is the length of the array. We examine all previous elements for each element.
- **Space Complexity**: \(O(n)\), for storing the `dp` array.

## Approach 2: Binary Search with Patience Sorting

### Intuition
This approach optimizes the search step using binary search and is inspired by the card game patience where we maintain piles of cards. The top of the piles helps to find the position of each card (or number in this context).

### Steps
1. Create an empty list `piles` to simulate the top cards of the piles.
2. For each number in the array, use binary search on `piles` to find the leftmost pile where the number can be placed:
   - If found, replace that pile's top with the number.
   - If not found, create a new pile with this number as the top.
3. The number of piles will be the length of the longest increasing subsequence.

### Code

```cpp
#include <vector>
#include <algorithm>

class Solution {
public:
    int lengthOfLIS(std::vector<int>& nums) {
        std::vector<int> piles;
        
        for (int x : nums) {
            // Use binary search to find the insertion point
            auto it = std::lower_bound(piles.begin(), piles.end(), x);
            if (it == piles.end()) {
                piles.push_back(x); // New pile is created
            } else {
                *it = x; // Replace the top of the pile
            }
        }
        
        return piles.size();
    }
};
```

### Complexity Analysis
- **Time Complexity**: \(O(n \log n)\), where \(n\) is the length of the array. Each insertion involves a binary search.
- **Space Complexity**: \(O(n)\), as we potentially simulate each element as a pile top.

