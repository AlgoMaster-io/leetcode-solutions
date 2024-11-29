# 354. [Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Approach 1: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <vector>
#include <algorithm>

class Solution {
public:
    int maxEnvelopes(std::vector<std::vector<int>>& envelopes) {
        if (envelopes.empty()) return 0;

        // Sort envelopes by width; if width is the same, sort by height descending
        std::sort(envelopes.begin(), envelopes.end(), [](const std::vector<int>& a, const std::vector<int>& b) {
            if (a[0] == b[0]) {
                return b[1] - a[1];
            } else {
                return a[0] - b[0];
            }
        });

        int n = envelopes.size();
        std::vector<int> dp(n, 1);

        int maxEnvelopes = 0;

        // Dynamic programming to find the maximum number of envelopes
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                // Check if j can fit into i
                if (envelopes[j][1] < envelopes[i][1] && envelopes[j][0] < envelopes[i][0]) {
                    dp[i] = std::max(dp[i], dp[j] + 1);
                }
            }
            maxEnvelopes = std::max(maxEnvelopes, dp[i]);
        }

        return maxEnvelopes;
    }
};
```

## Approach 2: Patience Sorting and Longest Increasing Subsequence (LIS)

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <vector>
#include <algorithm>

class Solution {
public:
    int maxEnvelopes(std::vector<std::vector<int>>& envelopes) {
        if (envelopes.empty()) return 0;

        // Sort envelopes by width; if width is the same, sort by height descending
        std::sort(envelopes.begin(), envelopes.end(), [](const std::vector<int>& a, const std::vector<int>& b) {
            if (a[0] == b[0]) {
                return b[1] - a[1];
            } else {
                return a[0] - b[0];
            }
        });

        // Vector to hold our increasing sequence of heights
        std::vector<int> lis;

        for (const auto& envelope : envelopes) {
            int height = envelope[1];

            // Perform binary search to find the correct position to replace or add
            auto pos = std::lower_bound(lis.begin(), lis.end(), height);

            // Add or replace the value at the identified position
            if (pos == lis.end()) {
                lis.push_back(height);
            } else {
                *pos = height;
            }
        }

        return lis.size();
    }
};
```

