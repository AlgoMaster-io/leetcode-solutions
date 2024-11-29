# 1049. [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

## Approach 1: Dynamic Programming with a Set

### Solution
```cpp
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
#include <unordered_set>
#include <vector>
#include <algorithm>

class Solution {
public:
    int lastStoneWeightII(std::vector<int>& stones) {
        std::unordered_set<int> dp;
        dp.insert(0);
        int sum = 0;

        // Iterate over each stone
        for (int stone : stones) {
            std::unordered_set<int> newDp;
            // Update possible sums with and without the current stone
            for (int s : dp) {
                newDp.insert(s + stone);
                newDp.insert(s - stone);
            }
            // Move to the new set of possible sums
            dp = std::move(newDp);
            sum += stone;
        }

        // Find the minimum possible non-negative sum (half the total sum)
        int minAbs = INT_MAX;
        for (int s : dp) {
            if (s >= 0) {
                minAbs = std::min(minAbs, s);
            }
        }
        
        return minAbs;
    }
};
```

## Approach 2: Dynamic Programming with Boolean Array

### Solution
```cpp
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
#include <vector>

class Solution {
public:
    int lastStoneWeightII(std::vector<int>& stones) {
        int sum = 0;
        for (int stone : stones) {
            sum += stone;
        }
        std::vector<bool> dp(sum / 2 + 1, false);
        dp[0] = true;

        // Evaluate possible subset sums
        for (int stone : stones) {
            for (int j = dp.size() - 1; j >= stone; j--) {
                dp[j] = dp[j] || dp[j - stone];
            }
        }

        // Find the largest possible subset sum that is <= sum/2
        for (int j = dp.size() - 1; j >= 0; j--) {
            if (dp[j]) {
                return sum - 2 * j;
            }
        }

        return 0;
    }
};
```

## Approach 3: Optimized DP with Single Array

### Solution
```cpp
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
#include <vector>
#include <algorithm>

class Solution {
public:
    int lastStoneWeightII(std::vector<int>& stones) {
        int sum = 0;
        for (int stone : stones) {
            sum += stone;
        }
        int target = sum / 2;
        std::vector<int> dp(target + 1, 0);

        // Calculate optimized subset sums
        for (int stone : stones) {
            for (int j = target; j >= stone; j--) {
                dp[j] = std::max(dp[j], dp[j - stone] + stone);
            }
        }

        // Result is the difference between total sum and twice the best subset sum
        return sum - 2 * dp[target];
    }
};
```

