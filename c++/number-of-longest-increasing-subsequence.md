# [673. Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Approaches
- [Dynamic Programming Approach](#dynamic-programming-approach)

---

### Dynamic Programming Approach

#### Intuition:
The problem is an extension of the longest increasing subsequence (LIS) problem. In addition to finding the length of the LIS, we are also required to count how many such subsequences exist. We can use a dynamic programming approach with two arrays:

1. `dp[i]`: This will store the length of the longest increasing subsequence ending at index `i`.
2. `count[i]`: This will store the number of longest increasing subsequences that end at index `i`.

#### Plan:
1. Initialize a `dp` array and set each value to 1, as the minimum length of LIS ending at any index is 1 (the element itself).
2. Initialize a `count` array with all values set to 1, because at least one subsequence exists, the element itself.
3. Traverse each element `nums[i]`, for each `j` from `0` to `i-1`, update `dp[i]` and `count[i]`:
   - If `nums[i] > nums[j]`, it means `nums[i]` can extend the subsequences ending at `j`.
   - If `dp[j] + 1 > dp[i]`, update `dp[i]` and set `count[i] = count[j]`.
   - If `dp[j] + 1 == dp[i]`, increase `count[i]` by `count[j]`.
4. After processing all elements, find the maximum value in `dp`, which gives the length of the longest increasing subsequences.
5. Sum up all `count[i]` where `dp[i]` equals the maximum length found in the previous step. This sum gives the number of such subsequences.

```cpp
#include <vector>
#include <algorithm>

int findNumberOfLIS(std::vector<int>& nums) {
    int n = nums.size();
    
    if (n == 0) return 0;
    
    // dp[i] to store length of LIS ending at index i
    std::vector<int> dp(n, 1);
    // count[i] to store number of LIS ending at index i
    std::vector<int> count(n, 1);
    
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < i; ++j) {
            if (nums[i] > nums[j]) {
                // If extending subsequence leads to a longer subsequence
                if (dp[j] + 1 > dp[i]) {
                    dp[i] = dp[j] + 1;
                    count[i] = count[j]; // Take the count from j
                } else if (dp[j] + 1 == dp[i]) {
                    // Add the count of subsequences ending at j
                    count[i] += count[j];
                }
            }
        }
    }
    
    // Find the length of the longest increasing subsequence
    int longest = *std::max_element(dp.begin(), dp.end());
    int result = 0;
    
    // Sum up counts of all subsequences with length equal to longest
    for (int i = 0; i < n; ++i) {
        if (dp[i] == longest) {
            result += count[i];
        }
    }
    
    return result;
}
```

#### Complexity Analysis
- **Time Complexity:** \( O(n^2) \), where \( n \) is the length of the input array. This results from the two nested loops where both `i` and `j` iterate over the array.
- **Space Complexity:** \( O(n) \), used by the `dp` and `count` arrays to store intermediate results.

